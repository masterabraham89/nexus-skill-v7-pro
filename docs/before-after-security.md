# Before / After — Security (OWASP)

> These examples show critical security vulnerabilities that code generated **without NEXUS-PRO** can contain, and how NEXUS-PRO eliminates them by default.

---

## Vulnerability 1: IDOR (Insecure Direct Object Reference)

This is the most common and dangerous vulnerability in multi-tenant applications.
**OWASP Top 10 — A01:2021 Broken Access Control**

### ❌ WITHOUT NEXUS-PRO

```php
// GET /api/invoices/{id}
public function show(int $id): InvoiceResource
{
    // ⚠️ CRITICAL: finds by ID only — no company_id check
    $invoice = Invoice::findOrFail($id);

    return new InvoiceResource($invoice);
}
```

**The attack:**
```http
# User from Company A authenticates and then requests:
GET /api/invoices/4521   ← invoice belonging to Company B

# Server returns Company B's invoice — full data exposure
# No error, no log — completely silent
```

Company A can iterate IDs (1, 2, 3, 4...) and read every invoice in the system.

---

### ✅ WITH NEXUS-PRO

```php
// GET /api/invoices/{id}
public function show(int $id, InvoiceRepository $repo): InvoiceResource
{
    // companyId always from authenticated token — never from URL or body
    $companyId = $this->request->user()->company_id;

    // findOrFail scoped to company — 404 if ID belongs to another company
    $invoice = $repo->findByIdForCompany($id, $companyId);

    if (!$invoice) {
        // ✅ Returns 404, not 403 — doesn't reveal the resource exists
        abort(404);
    }

    return new InvoiceResource($invoice);
}
```

```php
// InvoiceRepository.php
public function findByIdForCompany(int $id, int $companyId): ?Invoice
{
    return Invoice::query()
        ->where('id', $id)
        ->where('company_id', $companyId) // ✅ double-key filter
        ->first();
}
```

**NEXUS-PRO rule that prevents this:**
> `companyId` must ALWAYS come from the authenticated token (`$request->user()->company_id`),
> never from the URL, query string, or request body.
> Every repository query MUST include `where('company_id', $companyId)`.

---

## Vulnerability 2: Mass Assignment

**OWASP Top 10 — A03:2021 Injection / A04:2021 Insecure Design**

### ❌ WITHOUT NEXUS-PRO

```php
public function update(Request $request, User $user): UserResource
{
    // ⚠️ CRITICAL: fills ALL request data directly into the model
    $user->fill($request->all())->save();

    return new UserResource($user);
}
```

**The attack:**
```json
PATCH /api/users/42
{
  "name": "John",
  "email": "john@example.com",
  "role": "admin",          ← escalating to admin
  "company_id": 1,          ← switching company
  "is_verified": true       ← bypassing email verification
}
```

All fields get written. The attacker is now an admin.

---

### ✅ WITH NEXUS-PRO

```php
// Http/Requests/UpdateUserRequest.php
class UpdateUserRequest extends FormRequest
{
    public function rules(): array
    {
        return [
            // ✅ Only whitelisted fields — nothing else passes through
            'name'   => ['required', 'string', 'max:100'],
            'email'  => ['required', 'email', 'unique:users,email,' . $this->route('user')->id],
            'locale' => ['nullable', 'in:en,es,pt'],
        ];
        // role, company_id, is_verified — not listed → automatically rejected
    }
}
```

```php
public function update(UpdateUserRequest $request, User $user): UserResource
{
    // authorize() in FormRequest ensures user can only update their own profile
    // validated() returns ONLY the whitelisted fields
    $user->update($request->validated());

    return new UserResource($user);
}
```

**NEXUS-PRO rule that prevents this:**
> Always use `$request->validated()` (from FormRequest), never `$request->all()`.
> `FormRequest` defines the exact whitelist of allowed fields.
> Sensitive fields like `role`, `company_id`, `is_admin` are NEVER in FormRequest rules.

---

## Vulnerability 3: Sensitive Data in Logs

**OWASP Top 10 — A02:2021 Cryptographic Failures**

### ❌ WITHOUT NEXUS-PRO

```php
// Debugging a payment failure
Log::info('Payment attempt', [
    'user'       => $user->toArray(),     // ⚠️ includes password hash
    'request'    => $request->all(),      // ⚠️ includes card number, CVV
    'api_key'    => config('services.stripe.secret'), // ⚠️ secret key in logs
    'token'      => $request->bearerToken(), // ⚠️ auth token in logs
]);
```

**The attack:**
Anyone with access to log files (log aggregators, compromised monitoring tools, rogue employees) now has: password hashes, card numbers, API keys, and auth tokens.

---

### ✅ WITH NEXUS-PRO

```php
// ✅ Only safe, non-sensitive information in logs
Log::warning('payment.failed', [
    'user_id'    => $user->id,            // ID only — no personal data
    'company_id' => $user->company_id,
    'amount'     => $payment->amount,
    'currency'   => $payment->currency,
    'error_code' => $e->getCode(),        // code, not full exception
    'processor'  => 'stripe',
    // No: password, card number, CVV, API key, bearer token
]);
```

**NEXUS-PRO rule that prevents this:**
> Logs must NEVER contain: passwords, password hashes, card numbers, CVV, API keys,
> bearer tokens, session IDs, or full exception stack traces in production.
> Log IDs and codes — not raw data.

---

## Summary

| Vulnerability | Without NEXUS-PRO | With NEXUS-PRO |
|---------------|------------------|----------------|
| IDOR (read other company's data) | ❌ Silent leak | ✅ 404 + company-scoped queries |
| Mass Assignment (privilege escalation) | ❌ Any field writable | ✅ Whitelist-only via FormRequest |
| Sensitive data in logs | ❌ Tokens/cards logged | ✅ IDs and codes only |
| Broken Object Level Auth | ❌ ID from URL trusted | ✅ companyId from token always |
| Missing auth on endpoints | ❌ Sometimes forgotten | ✅ FormRequest.authorize() enforced |

> NEXUS-PRO applies OWASP Top 10 checks on every generated endpoint — security is not optional.
