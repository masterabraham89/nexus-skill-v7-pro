# NEXUS-PRO Roadmap

This document shows the planned direction for NEXUS-PRO. Items are not guaranteed and may change based on community feedback.

> Want to influence the roadmap? Open a [Feature Request](./.github/ISSUE_TEMPLATE/feature_request.yml).

---

## ✅ Completed — v7.0-pro-enterprise

- [x] Core architecture rules (Controller → Service → Repository)
- [x] Multi-tenant isolation enforcement (`company_id` from token)
- [x] OWASP Top 10 security checklist
- [x] 55 technical references
- [x] 13 ready-to-use prompts
- [x] 15 code templates
- [x] Plan Before Code gate
- [x] Definition of Done checklist
- [x] Feature flags reference
- [x] Audit trail patterns
- [x] Zero-downtime migration patterns
- [x] Idempotent endpoint patterns
- [x] Export safety patterns
- [x] **Bilingual support (EN + ES)**
- [x] **Public README + CONTRIBUTING guide**
- [x] **GitHub community files (.github/)**
- [x] **CC BY-NC 4.0 license**
- [x] **Before/After examples (backend, frontend, security)**
- [x] **NEXUS_CONFIG.md project configuration system**
- [x] **Comparison vs other skills**
- [x] **IndexedDB + Offline Sync reference**
- [x] **DataGuard pattern reference**
- [x] **WebSocket + Real-Time patterns**
- [x] **Error Recovery + Circuit Breaker patterns**
- [x] **Capacitor + PWA Advanced patterns**

---

## 🚧 In Progress — v7.1

- [ ] **Portuguese translation** (`README.pt.md`) — for Brazilian developer community
- [ ] **GraphQL reference** — patterns for Laravel + Lighthouse or NestJS + Apollo
- [ ] **Stripe / Payment processor patterns** — idempotency, webhooks, reconciliation
- [ ] **Rate limiting reference** — per-user, per-company, per-endpoint throttling

---

## 📋 Planned — v7.2

- [ ] **Multi-language content (i18n) patterns** — when `features.i18n: true`
- [ ] **Notification system reference** — in-app, push, email queue patterns
- [ ] **File upload advanced patterns** — chunked upload, virus scanning, CDN delivery
- [ ] **Search patterns** — full-text search with PostgreSQL or Meilisearch
- [ ] **Webhook patterns** — receiving and sending webhooks safely

---

## 🔭 Future — v8.0

- [ ] **Django + DRF backend support** — for Python projects
- [ ] **Next.js App Router frontend support** — full SSR/RSC patterns
- [ ] **Kubernetes / Docker deployment reference** — containerized production
- [ ] **Multi-database patterns** — PostgreSQL + Redis + S3 coordination
- [ ] **AI integration patterns** — safely integrating LLM APIs into enterprise apps
- [ ] **Interactive configuration wizard** — CLI that generates `NEXUS_CONFIG.md`
- [ ] **Automated skill validation** — script to verify skill integrity before release

---

## 💡 Community Requested

These were requested by users and are under evaluation:

| Request | Status | Issue |
|---------|--------|-------|
| Laravel Octane patterns | Evaluating | — |
| Vue 3 Composition API deep reference | Evaluating | — |
| React Native support | Under discussion | — |
| MongoDB patterns | Under discussion | — |

---

## Version Policy

| Version type | What it means |
|-------------|---------------|
| `7.0` → `7.1` | New references, prompts, or templates added |
| `7.1` → `7.2` | New stack support or significant rule changes |
| `7.x` → `8.0` | Major architectural shift or new core behavior |

Versions are incremented in `SKILL.md`, `MANIFEST.md`, `CHANGELOG.md`, and README badges simultaneously.

---

## How to Influence the Roadmap

1. **Vote with 👍** on existing Feature Request issues to signal priority
2. **Open a new Feature Request** with a clear use case
3. **Submit a PR** — accepted contributions move immediately to the next release
4. **Share your experience** — tell us what's missing in your real projects

The roadmap reflects what real enterprise developers need, not theoretical completeness.
