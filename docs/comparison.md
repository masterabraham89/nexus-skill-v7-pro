# NEXUS-PRO vs Other Antigravity Skills

## Why This Comparison Matters

Antigravity's skill ecosystem has dozens of engineering skills. This document explains the specific positioning of NEXUS-PRO and when to choose it over (or alongside) other options.

---

## Direct Comparison

### NEXUS-PRO vs `backend-architect`

| Feature | `backend-architect` | **NEXUS-PRO** |
|---------|--------------------|--------------| 
| Focus | Architecture decisions and patterns (generic) | Architecture + implementation (Laravel/React/PostgreSQL specific) |
| Stack-specific rules | ❌ Framework-agnostic | ✅ Laravel Eloquent, Sanctum, FormRequest, Policy, Queues |
| Multi-tenant isolation | ⚠️ Mentioned as concept | ✅ Enforced with `company_id` rules in every query |
| Security checklist | ⚠️ General principles | ✅ OWASP-mapped, per-endpoint checklist |
| Ready-to-use prompts | ❌ None | ✅ 13 prompts |
| Code templates | ❌ None | ✅ 15 templates |
| Offline sync patterns | ❌ Not covered | ✅ IndexedDB + SyncOrchestrator |
| Real-time patterns | ❌ Not covered | ✅ Pusher + WebSocket lifecycle |

**Use `backend-architect` when:** you're evaluating architecture trade-offs (microservices vs monolith, event-driven vs REST) before starting a project.

**Use NEXUS-PRO when:** your architecture is decided and you need to generate production-quality Laravel + React code consistently.

---

### NEXUS-PRO vs `senior-fullstack`

| Feature | `senior-fullstack` | **NEXUS-PRO** |
|---------|--------------------|--------------| 
| Scope | Broad full-stack patterns | Deep enterprise patterns for a specific stack |
| Tenant isolation rules | ⚠️ General | ✅ Specific: `company_id` from token, per-query enforcement |
| Financial audit trail | ❌ Not covered | ✅ Dedicated pattern and template |
| Idempotency | ❌ Not covered | ✅ `idempotency_key` pattern for payments/imports |
| Definition of Done | ❌ Not explicit | ✅ Checklist enforced before every delivery |
| Plan Before Code gate | ❌ Not enforced | ✅ Mandatory — Antigravity plans before writing |
| DataGuard pattern | ❌ Not covered | ✅ Full implementation |
| Production Gate | ❌ Not covered | ✅ Pre-delivery security checklist |

**Use `senior-fullstack` when:** you want general senior engineering guidance across any stack.

**Use NEXUS-PRO when:** you're building a Laravel + React application and need surgical, stack-specific rules that prevent real production bugs.

---

### NEXUS-PRO vs `security-auditor`

| Feature | `security-auditor` | **NEXUS-PRO** |
|---------|--------------------|--------------| 
| Focus | Audit existing code for vulnerabilities | Generate secure code from the start |
| OWASP coverage | ✅ Comprehensive | ✅ Applied preventively per pattern |
| Laravel-specific vulnerabilities | ⚠️ Generic | ✅ Sanctum, FormRequest, mass assignment |
| Multi-tenant IDOR | ⚠️ General IDOR | ✅ company_id-specific prevention pattern |
| Integration with code generation | ❌ Audit only | ✅ Security built into every generated file |

**Use `security-auditor` when:** you need to audit a codebase that already exists.

**Use NEXUS-PRO when:** you want security enforced from the first line of code — prevention, not remediation.

**Use both:** run `security-auditor` after a sprint to catch any gaps, while NEXUS-PRO prevents them during development.

---

### NEXUS-PRO vs `api-patterns`

| Feature | `api-patterns` | **NEXUS-PRO** |
|---------|---------------|--------------| 
| Focus | REST vs GraphQL trade-offs, pagination, versioning | Full API implementation with auth, ownership, transactions |
| Implementation depth | ⚠️ Conceptual | ✅ Working code patterns |
| Laravel FormRequest | ❌ Not covered | ✅ Template included |
| Laravel Resource/Collection | ❌ Not covered | ✅ Template included |
| Auth integration | ⚠️ Mentioned | ✅ Sanctum + middleware enforced |

**Use `api-patterns` when:** you're designing an API contract before implementation.

**Use NEXUS-PRO when:** you're implementing the API and need patterns that prevent auth bugs and data leaks.

---

### NEXUS-PRO vs `database-design`

| Feature | `database-design` | **NEXUS-PRO** |
|---------|------------------|--------------| 
| Focus | Schema design, indexing strategy, ORM selection | Safe migrations, upserts, multi-tenant queries, audit parity |
| Laravel migrations | ⚠️ General | ✅ Zero-downtime migration patterns |
| Multi-tenant schema | ⚠️ Concept | ✅ `company_id` enforcement on every table |
| Chunked upserts | ❌ Not covered | ✅ 200-record chunk rule |
| Financial audit parity | ❌ Not covered | ✅ Dedicated reference |

**Use `database-design` when:** choosing your database architecture (PostgreSQL vs MongoDB, normalized vs denormalized).

**Use NEXUS-PRO when:** you're writing migrations and queries for your Laravel + PostgreSQL project.

---

## When to Use NEXUS-PRO Alone vs Combined

### Use NEXUS-PRO alone (80% of cases):
- Building a new feature in an existing Laravel + React project
- Debugging a backend endpoint or frontend component
- Writing migrations, services, repositories, hooks, or components
- Reviewing security of a new PR

### Combine NEXUS-PRO with other skills:

| Combination | When |
|-------------|------|
| NEXUS-PRO + `security-auditor` | Monthly security audit sprints |
| NEXUS-PRO + `backend-architect` | Starting a new project (architecture first, then implementation) |
| NEXUS-PRO + `performance-engineer` | Optimizing slow endpoints after feature delivery |
| NEXUS-PRO + `tdd-workflow` | When writing tests for NEXUS-PRO generated code |
| NEXUS-PRO + `database-design` | Designing a new database schema before writing migrations |

---

## The NEXUS-PRO Advantage in One Sentence

> Other skills teach you **what** to do.  
> NEXUS-PRO makes Antigravity actually **do it** — consistently, in the right stack, with security and architecture enforced on every file.

---

## Feature Coverage Matrix

| Capability | `backend-architect` | `senior-fullstack` | `security-auditor` | `api-patterns` | **NEXUS-PRO** |
|------------|--------------------|--------------------|--------------------|----|------|
| Laravel patterns | ❌ | ⚠️ | ❌ | ❌ | ✅ |
| React/TypeScript patterns | ❌ | ⚠️ | ❌ | ❌ | ✅ |
| Multi-tenant isolation | ⚠️ | ⚠️ | ⚠️ | ❌ | ✅ |
| OWASP enforcement | ⚠️ | ❌ | ✅ | ❌ | ✅ |
| Offline sync / IndexedDB | ❌ | ❌ | ❌ | ❌ | ✅ |
| Real-time / WebSocket | ❌ | ❌ | ❌ | ❌ | ✅ |
| Financial audit | ❌ | ❌ | ❌ | ❌ | ✅ |
| Ready-to-use prompts | ❌ | ❌ | ❌ | ❌ | ✅ 13 |
| Code templates | ❌ | ❌ | ❌ | ❌ | ✅ 15 |
| 55 technical references | ❌ | ❌ | ❌ | ❌ | ✅ |
| Bilingual (EN + ES) | ❌ | ❌ | ❌ | ❌ | ✅ |
| Project config system | ❌ | ❌ | ❌ | ❌ | ✅ |
| Definition of Done | ❌ | ❌ | ❌ | ❌ | ✅ |
| Plan Before Code gate | ❌ | ❌ | ❌ | ❌ | ✅ |
