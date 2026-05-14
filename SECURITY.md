# Security Policy — NEXUS-PRO

> Licensed under [CC BY-NC 4.0](../LICENSE) — Free for non-commercial use.

## Supported Versions

| Version | Supported |
|---------|-----------|
| 7.0-pro-enterprise | ✅ Active |
| 6.0-enterprise | ⚠️ Critical fixes only |
| 5.0-ultimate | ❌ No longer supported |

---

## Reporting a Security Vulnerability

NEXUS-PRO is a documentation skill — it does not execute code or handle data directly.
However, if you find a security issue in any of the following areas, please report it responsibly:

- A rule or template that generates **inherently insecure code** (e.g., SQL injection vulnerability, missing auth, IDOR pattern)
- An example that **exposes sensitive data patterns**
- A reference document that gives **incorrect security guidance**

### How to Report

**Do NOT open a public GitHub issue for security vulnerabilities.**

Instead, please:

1. Email the author directly: `abraham@antigravity.dev` *(replace with your actual email)*
2. Use the subject line: `[NEXUS-PRO SECURITY] Brief description`
3. Include:
   - The affected file(s)
   - A description of the insecure rule or pattern
   - The correct behavior it should recommend instead

### Response Time

We aim to acknowledge reports within **48 hours** and release a fix within **7 days** for critical issues.

---

## Security Philosophy

NEXUS-PRO is built on the principle that **security must be designed in, not added later**.
Every rule in this skill enforces:

- Authentication before action
- Authorization with ownership validation
- No sensitive data in logs
- Input validation on every boundary
- OWASP Top 10 coverage

If a rule in this skill contradicts these principles, that is a security bug.
