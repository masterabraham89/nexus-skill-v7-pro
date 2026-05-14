# Before / After — Backend (Laravel)

> These examples show the difference between code generated **without NEXUS-PRO** and code generated **with NEXUS-PRO** active.

---

## Example 1: Creating an Order Endpoint

### ❌ WITHOUT NEXUS-PRO

Antigravity generates a single controller method that does everything:

```php
// OrderController.php — everything in one place
public function store(Request $request)
{
    // No FormRequest — validation inline and incomplete
    $request->validate([
        'items' => 'required',
    ]);

    // Direct DB query in controller — breaks separation of concerns
    $order = Order::create([
        'user_id'    => $request->user_id,  // ⚠️ IDOR risk: trusting client data
        'company_id' => $request->company_id, // ⚠️ CRITICAL: company_id from client!
        'items'      => $request->items,
        'total'      => $request->total,
    ]);

    // No transaction — if something fails after this, data is corrupted
    // No ownership validation
    // No audit trail
    // No error handling

    return response()->json($order);
}
```

**Problems in this code:**
- `company_id` comes from the client → any user can create orders for ANY company
- No transaction → partial writes possible
- No FormRequest → validation incomplete
- No Policy/Gate → no authorization
- No audit trail → no traceability
- Controller has business logic → untestable, unmaintainable

---

### ✅ WITH NEXUS-PRO

Antigravity generates a full layered architecture:

```php
// Http/Requests/StoreOrderRequest.php
class StoreOrderRequest extends FormRequest
{
    public function authorize(): bool
    {
        return $this->user()->can('create', Order::class);
    }

    public function rules(): array
    {
        return [
            'items'              => ['required', 'array', 'min:1', 'max:50'],
            'items.*.product_id' => ['required', 'integer', 'exists:products,id'],
            'items.*.quantity'   => ['required', 'integer', 'min:1', 'max:999'],
            'items.*.price'      => ['required', 'numeric', 'min:0', 'max:99999.99'],
            'notes'              => ['nullable', 'string', 'max:500'],
        ];
    }
}
```

```php
// Http/Controllers/OrderController.php — thin, delegates everything
class OrderController extends Controller
{
    public function store(StoreOrderRequest $request, OrderService $service): OrderResource
    {
        $order = $service->create(
            user: $request->user(),
            data: $request->validated()
        );

        return new OrderResource($order);
    }
}
```

```php
// Services/OrderService.php — business logic here only
class OrderService
{
    public function create(User $user, array $data): Order
    {
        // companyId from authenticated token — never from client
        $companyId = $user->company_id;

        return DB::transaction(function () use ($user, $companyId, $data) {
            $order = $this->orders->createForCompany($companyId, [
                'user_id'    => $user->id,
                'company_id' => $companyId,
                'notes'      => $data['notes'] ?? null,
                'status'     => OrderStatus::PENDING,
            ]);

            $this->orders->attachItems($order, $data['items']);
            $this->audit->record('order.created', $order, $user);

            return $order->load('items');
        });
    }
}
```

```php
// Repositories/OrderRepository.php — queries only
class OrderRepository
{
    public function createForCompany(int $companyId, array $data): Order
    {
        return Order::create(array_merge($data, ['company_id' => $companyId]));
    }

    public function findByIdForCompany(int $id, int $companyId): ?Order
    {
        return Order::query()
            ->where('id', $id)
            ->where('company_id', $companyId) // ✅ IDOR prevention
            ->first();
    }
}
```

**What NEXUS-PRO added automatically:**
- ✅ `company_id` from authenticated token — IDOR eliminated
- ✅ `DB::transaction` with automatic rollback
- ✅ `FormRequest` with complete validation and limits
- ✅ `Policy` authorization before any action
- ✅ Audit trail on every write
- ✅ Controller with zero business logic — fully testable
- ✅ Repository with ownership filter on every query

---

## Example 2: Bulk Product Sync (Upsert)

### ❌ WITHOUT NEXUS-PRO

```php
// Syncing 5000 products in one shot
public function sync(array $products): void
{
    // Dumps 5000 records in a single query — memory explosion + timeout
    Product::upsert(
        $products,
        ['external_id'],
        ['name', 'price', 'stock']
    );
}
```

**Problems:**
- 5000 records in one query → memory error or timeout
- No idempotency key → running twice duplicates data
- No company_id filter → data leak between companies
- No error handling → silent failure

---

### ✅ WITH NEXUS-PRO

```php
public function sync(int $companyId, array $products): SyncResult
{
    $processed = 0;
    $failed    = 0;

    // Deduplicate by external_id before processing
    $unique = collect($products)
        ->unique('external_id')
        ->values();

    // Process in chunks of 200 — safe for memory and DB
    $unique->chunk(200)->each(function ($chunk) use ($companyId, &$processed, &$failed) {
        try {
            Product::upsert(
                $chunk->map(fn($p) => array_merge($p, [
                    'company_id' => $companyId, // ✅ always scoped
                    'synced_at'  => now(),
                ]))->toArray(),
                ['external_id', 'company_id'], // unique keys
                ['name', 'price', 'stock', 'synced_at']
            );
            $processed += $chunk->count();
        } catch (\Exception $e) {
            Log::warning('sync.chunk.failed', [
                'company_id' => $companyId,
                'chunk_size' => $chunk->count(),
                'error'      => $e->getMessage(), // no stack trace exposed
            ]);
            $failed += $chunk->count();
        }
    });

    return new SyncResult($processed, $failed);
}
```

**What NEXUS-PRO added:**
- ✅ Chunks of 200 — safe for any dataset size
- ✅ Deduplication before upsert
- ✅ `company_id` always scoped — multi-tenant safe
- ✅ Per-chunk error handling — one bad chunk doesn't kill the whole sync
- ✅ Structured logs without sensitive data

---

## Summary

| Metric | Without NEXUS-PRO | With NEXUS-PRO |
|--------|------------------|----------------|
| Files generated | 1 (giant controller) | 4 (Controller, Request, Service, Repository) |
| IDOR vulnerability | ❌ Present | ✅ Eliminated |
| Transaction safety | ❌ None | ✅ Full rollback |
| Authorization | ❌ Missing | ✅ Policy enforced |
| Bulk sync safety | ❌ Memory risk | ✅ Chunked + deduplicated |
| Testability | ❌ Hard | ✅ Each layer independently testable |
| Audit trail | ❌ None | ✅ Every write logged |
