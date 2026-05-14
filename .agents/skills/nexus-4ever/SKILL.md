---
name: nexus-skill-v7-pro-enterprise
description: "Skill enterprise para Antigravity orientado al desarrollo full stack robusto, seguro, escalable y mantenible. Aplicable a cualquier proyecto que use Laravel/PHP, React/TypeScript y PostgreSQL en entornos SaaS, multiempresa, POS, e-commerce o paneles administrativos."
version: "7.0-pro-enterprise"
author: "Abraham Apeña Rosas"
company: "antigravity"
skill_name: "NEXUS-PRO"
full_name: "NEXUS-PRO: Neural Expert for Unified eXtended Systems - Enterprise Edition"
platform: "Antigravity"
category: "enterprise-software-engineering"
target_projects:
  - "Plataformas SaaS multi-tenant"
  - "Sistemas de Punto de Venta (POS)"
  - "Paneles administrativos enterprise"
  - "E-commerce con backend Laravel"
  - "APIs REST con React/TypeScript frontend"
  - "Sistemas con roles y permisos por empresa"
  - "Aplicaciones con sincronización offline/online"
  - "Proyectos con PostgreSQL y migraciones críticas"
  - "Sistemas con auditoría y trazabilidad financiera"
  - "Apps PWA/Capacitor con backend cloud"
tags:
  - laravel
  - php
  - react
  - typescript
  - postgresql
  - controller-service-repository
  - security
  - performance
  - testing
  - cicd
  - monitoring
  - scalability
  - privacy
  - openapi
  - ddd-lite
  - observability
  - migration-safety
  - production-readiness
  - tenant-isolation
  - feature-flags
  - idempotency
  - audit-trail
  - dependency-governance
  - infrastructure
  - definition-of-done
  - saas
  - pos
  - e-commerce
  - pwa
  - capacitor
triggers:
  # — Español —
  - "crear endpoint"
  - "refactorizar backend"
  - "refactorizar frontend"
  - "crear módulo React"
  - "crear servicio Laravel"
  - "crear repository"
  - "crear controller"
  - "crear hook"
  - "crear modal"
  - "revisar seguridad"
  - "optimizar performance"
  - "debug API"
  - "preparar despliegue"
  - "auditar código"
  - "revisar aislamiento multiempresa"
  - "crear export"
  - "subir archivos"
  - "crear endpoint idempotente"
  - "preparar release"
  # — English —
  - "create endpoint"
  - "refactor backend"
  - "refactor frontend"
  - "create React module"
  - "create Laravel service"
  - "create repository"
  - "create controller"
  - "create hook"
  - "create modal"
  - "review security"
  - "security audit"
  - "optimize performance"
  - "debug API"
  - "prepare deployment"
  - "audit code"
  - "review multi-tenant isolation"
  - "create export"
  - "file upload"
  - "create idempotent endpoint"
  - "prepare release"
  - "production release"
  - "code review"
  - "architecture review"
---

# NEXUS-PRO

**NEXUS-PRO: Neural Expert for Unified eXtended Systems - Enterprise Edition** es un skill enterprise para guiar a Antigravity en el desarrollo profesional de proyectos full stack reales, con énfasis en arquitectura limpia, seguridad moderna, rendimiento, mantenibilidad, escalabilidad, testing, monitoreo, privacidad y despliegue controlado.

---

## ⚙️ Project Configuration (Auto-Read)

**At the start of every session, Antigravity MUST:**

1. Look for a `NEXUS_CONFIG.md` file in the root of the current project.
2. If found, read it and adapt ALL rules and code generation to match the declared stack.
3. If NOT found, use the default stack: `Laravel + React + TypeScript + PostgreSQL + Sanctum + Redis`.
4. Announce the detected stack to the user: *"NEXUS-PRO loaded. Stack: [detected values]."*

**Stack adaptation rules:**

| NEXUS_CONFIG value | Adaptation |
|--------------------|-----------|
| `backend.framework: nestjs` | Generate NestJS modules/controllers/services/guards instead of Laravel |
| `backend.database.primary: mysql` | Use MySQL syntax (no UUID default, use BIGINT, adapt migrations) |
| `backend.auth.driver: jwt` | Use JWT middleware patterns instead of Sanctum `auth:sanctum` |
| `backend.orm: typeorm` | Generate TypeORM entities/repositories instead of Eloquent models |
| `frontend.framework: vue` | Generate Vue 3 Composition API instead of React hooks |
| `frontend.state.client: pinia` | Use Pinia stores instead of Zustand |
| `frontend.state.server: vue-query` | Use Vue Query instead of SWR |
| `project.tenant_key: org_id` | Use `org_id` everywhere instead of `company_id` |
| `project.multi_tenant: false` | Skip tenant isolation rules — single-tenant project |
| `features.offline_sync: true` | Apply IndexedDB + SyncOrchestrator patterns |
| `features.financial_audit: true` | Enforce strict audit trail on all financial operations |
| `realtime.enabled: true` | Apply Pusher/WebSocket patterns for real-time features |

**If no `NEXUS_CONFIG.md` exists**, suggest the user create one:

> *"No NEXUS_CONFIG.md found. Using default stack (Laravel + React + PostgreSQL).  
> To customize the skill for your project, copy NEXUS_CONFIG.template.md to your project root."*

---


## Proyectos para los que aplica

Este skill está diseñado para cualquier proyecto que comparta una o más de estas características:

- **SaaS multi-tenant**: plataformas donde múltiples empresas o clientes comparten infraestructura con aislamiento estricto de datos.
- **Sistemas POS (Punto de Venta)**: aplicaciones de ventas con sincronización offline/online, catálogos, inventario, auditoría financiera y reportes.
- **E-commerce con backend Laravel**: tiendas con carrito, órdenes, pagos, integraciones y paneles de administración.
- **Paneles administrativos enterprise**: dashboards con roles, permisos, reportes, exports y gestión de múltiples entidades.
- **APIs REST con frontend React/TypeScript**: arquitecturas desacopladas donde el backend es exclusivamente API y el frontend es SPA.
- **Apps con sincronización crítica**: sistemas que operan parcialmente offline y sincronizan con base de datos cloud.
- **Proyectos con auditoría financiera o trazabilidad**: donde cada operación debe quedar registrada con actor, empresa, timestamp y estado.
- **PWA o Apps Capacitor/Cordova**: aplicaciones web progresivas o híbridas que corren en móvil y desktop.
- **Proyectos con PostgreSQL y migraciones frecuentes**: cualquier sistema donde la base de datos evoluciona en producción sin downtime.

Este skill debe usarse como norma operativa principal cuando Antigravity trabaje sobre cualquier desarrollo relacionado con Laravel, PHP, React, TypeScript, PostgreSQL, APIs, módulos administrativos, sincronizaciones, autenticación, autorización, integraciones, reporting, PWA, procesos batch o sistemas multiempresa.

---

## Cuándo debe activarse este skill

Antigravity debe activar este skill cuando el usuario solicite cualquiera de estas tareas:

- Crear, refactorizar o corregir código backend en Laravel / PHP.
- Crear, refactorizar o corregir código frontend en React / TypeScript.
- Diseñar endpoints, servicios, repositorios, jobs, policies, gates o FormRequests.
- Crear módulos frontend con carpetas `modules`, `hooks`, `components`, `modals`, `services` y `types`.
- Implementar autenticación, autorización, roles, permisos o ownership.
- Trabajar con PostgreSQL, migraciones, índices, queries, transacciones o reportes.
- Optimizar performance backend, frontend o base de datos.
- Corregir bugs en APIs, sincronizaciones, dashboards o componentes.
- Implementar procesos de importación, upsert, sync, deduplicación o jobs pesados.
- Preparar despliegue, CI/CD, testing, observabilidad o monitoreo.
- Revisar seguridad, privacidad, cumplimiento normativo o exposición de datos.
- Diseñar features nuevas que deban ser mantenibles y escalables.

---

## Objetivo operativo del skill

El objetivo es que Antigravity programe como un arquitecto de software enterprise senior, evitando soluciones improvisadas, archivos gigantes, lógica mezclada, consultas inseguras, componentes acoplados, validación insuficiente, endpoints sin autorización, duplicación de reglas de negocio o features difíciles de mantener.

Toda entrega debe priorizar:

1. Correctitud funcional.
2. Seguridad por defecto.
3. Separación estricta de responsabilidades.
4. Escalabilidad progresiva.
5. Rendimiento verificable.
6. Código legible y testeable.
7. Bajo acoplamiento.
8. Alta cohesión.
9. Observabilidad.
10. Preparación para producción.

---

## Stack principal

Antigravity debe asumir el siguiente stack base salvo que el usuario indique otro:

### Backend

- Laravel / PHP.
- Arquitectura `Controller -> Service -> Repository`.
- FormRequest para validación.
- Sanctum para autenticación API.
- Gates y Policies para autorización.
- Jobs y queues para procesos pesados.
- PostgreSQL como base de datos principal.
- Redis para cache, locks, colas y rate limiting.
- Logs estructurados con contexto.
- Transacciones con rollback obligatorio cuando existan operaciones críticas.

### Frontend

- React.
- TypeScript.
- Estructura modular por dominio.
- `useSWR` o `react-query` para datos remotos.
- Hooks sin JSX.
- Componentes puros y reutilizables.
- Modales independientes.
- Servicios frontend para llamadas HTTP.
- Tipos compartidos por módulo.
- Manejo explícito de loading, error, empty state y success state.

### Base de datos

- PostgreSQL.
- Prepared statements mediante ORM/query builder.
- Índices diseñados según filtros reales.
- `EXPLAIN ANALYZE` en queries críticas.
- Transacciones para consistencia.
- Upserts en chunks de 200.
- Auditoría cuando haya cambios sensibles.
- Mínimos privilegios por usuario/conexión.

---

## Reglas de oro obligatorias

### Arquitectura general

- Una responsabilidad por archivo.
- Evitar archivos gigantes.
- Dividir lógica por dominio, caso de uso y capa.
- No mezclar presentación, negocio, persistencia y transporte.
- No duplicar reglas de negocio entre backend y frontend.
- No crear helpers genéricos sin propósito claro.
- No crear abstracciones prematuras.
- No entregar código sin validar seguridad.

### Backend Laravel

- Controllers no tocan DB.
- Controllers solo reciben request, delegan al service y retornan response.
- Services contienen reglas de negocio.
- Repositories contienen queries.
- FormRequest valida entrada.
- Policies/Gates validan autorización.
- Jobs ejecutan procesos pesados.
- Events/Listeners se usan para efectos secundarios desacoplados.
- Las transacciones deben tener rollback.
- Los errores deben manejarse con excepciones controladas.
- Los logs nunca deben exponer secretos, tokens, contraseñas o datos sensibles innecesarios.
- `companyId` siempre debe obtenerse desde el token autenticado o contexto seguro del usuario autenticado.
- Nunca confiar en `company_id` enviado por el cliente.
- Siempre verificar ownership antes de leer, modificar o eliminar datos.
- Upserts en chunks de 200.
- Sync con deduplicación obligatoria.
- Correlativos deben generarse de forma transaccional o con lock seguro.
- Procesos batch deben ser idempotentes.

### Frontend React

- Components no hacen fetch directo.
- Hooks no contienen JSX.
- Modales independientes.
- Servicios frontend encapsulan llamadas HTTP.
- `useSWR` o `react-query` para datos remotos.
- `useEffect` debe tener cleanup obligatorio cuando use listeners, timers, subscriptions, AbortController o side effects persistentes.
- Evitar props drilling excesivo.
- Tipar props, responses y payloads.
- Cada módulo debe tener `types`.
- Componentes deben manejar loading, error, empty state y success state.
- No guardar tokens sensibles en lugares inseguros.
- No exponer claves privadas en frontend.
- Evitar renders innecesarios mediante memoización cuando exista evidencia de impacto.
- No usar `any` salvo justificación excepcional.

### Seguridad

- Revisar seguridad antes de entregar.
- Validar ownership.
- Validar autorización.
- Validar entrada.
- Validar límites de líneas y tamaño de archivos.
- Validar rate limits en endpoints sensibles.
- Evitar IDOR.
- Evitar Mass Assignment.
- Evitar SQL Injection.
- Evitar XSS.
- Evitar CSRF.
- Evitar exposición de API keys.
- Sanitizar salida cuando aplique.
- Registrar auditoría en acciones sensibles.
- No usar datos de producción en desarrollo.
- No registrar datos sensibles en logs.
- No devolver trazas internas al cliente.

---

## Límites por archivo

Antigravity debe respetar límites razonables de tamaño:

| Tipo | Límite recomendado |
|---|---:|
| Controller | 80-150 líneas |
| Service | 150-300 líneas |
| Repository | 120-250 líneas |
| FormRequest | 40-120 líneas |
| Policy | 40-150 líneas |
| React component | 80-180 líneas |
| Hook | 40-140 líneas |
| Modal | 80-180 líneas |
| Service frontend | 40-140 líneas |
| Type file | sin lógica, solo contratos |

Si un archivo excede el límite, Antigravity debe proponer división antes de continuar.

---

## Estructura backend recomendada

```text
app/
├── Http/
│   ├── Controllers/
│   ├── Requests/
│   └── Resources/
├── Services/
├── Repositories/
├── Policies/
├── Actions/
├── Jobs/
├── Events/
├── Listeners/
├── DTOs/
├── Enums/
└── Exceptions/
```

### Flujo backend obligatorio

```text
Request
  -> FormRequest
  -> Controller
  -> Policy/Gate
  -> Service
  -> Repository
  -> Database
  -> Resource/Response
```

El controller no debe contener reglas de negocio ni queries. El service no debe depender directamente del request HTTP. El repository no debe contener decisiones de negocio.

---

## Estructura frontend recomendada

```text
src/
├── modules/
│   └── module-name/
│       ├── components/
│       ├── hooks/
│       ├── modals/
│       ├── services/
│       ├── types/
│       ├── utils/
│       └── pages/
├── shared/
│   ├── components/
│   ├── hooks/
│   ├── services/
│   ├── types/
│   └── utils/
└── app/
```

### Flujo frontend obligatorio

```text
Page
  -> Module Component
  -> Hook
  -> Service
  -> API
```

Los componentes no hacen fetch directo. Los hooks orquestan estado y datos. Los services ejecutan llamadas HTTP. Los types definen contratos.

---

## Workflow

Antigravity debe seguir este flujo antes de entregar cualquier implementación:

### 1. Analizar

- Entender el requerimiento funcional.
- Identificar dominio, entidades, usuarios, permisos, datos y flujos.
- Revisar archivos existentes antes de crear nuevos.
- Detectar impactos colaterales.
- Identificar riesgos de seguridad, performance y consistencia.

### 2. Dividir responsabilidades

- Separar controller, service, repository, request, policy y resource.
- Separar page, component, hook, modal, service y types.
- Evitar lógica mixta.
- Definir contratos claros entre capas.
- Confirmar que cada archivo tenga una responsabilidad.

### 3. Implementar

- Codificar cambios mínimos y coherentes.
- Reutilizar patrones existentes.
- Mantener nombres explícitos.
- Evitar duplicación.
- Usar transacciones donde corresponda.
- Aplicar chunks de 200 en upserts o imports.
- Implementar deduplicación en sync.
- Obtener `companyId` desde token autenticado.

### 4. Validar seguridad

- Verificar autenticación.
- Verificar autorización.
- Verificar ownership.
- Verificar validación de entrada.
- Verificar protección contra IDOR.
- Verificar Mass Assignment.
- Verificar exposición de datos.
- Verificar logs seguros.
- Verificar rate limits.
- Verificar que no haya secretos en código.

### 5. Validar performance

- Revisar queries.
- Evitar N+1.
- Validar índices.
- Usar paginación.
- Evitar cargas masivas en memoria.
- Usar cache cuando sea seguro.
- Usar jobs para procesos pesados.
- Revisar renders innecesarios.
- Aplicar lazy loading/code splitting si corresponde.

### 6. Revisar checklist

- Aplicar checklist técnico.
- Aplicar checklist de seguridad.
- Aplicar checklist de testing.
- Aplicar checklist de accesibilidad si hay UI.
- Aplicar checklist de despliegue si el cambio llega a producción.

### 7. Entregar

- Explicar cambios realizados de forma técnica y concisa.
- Indicar archivos modificados.
- Indicar riesgos o supuestos.
- Indicar pruebas realizadas o recomendadas.
- No ocultar limitaciones.
- No entregar código inseguro como si estuviera listo para producción.

---

## Criterios de rechazo interno

Antigravity debe rechazar o corregir una implementación antes de entregarla si detecta:

- Controller con queries directas.
- Service acoplado a Request HTTP.
- Repository con reglas de negocio.
- Component con fetch directo.
- Hook con JSX.
- Modal mezclado dentro de una página enorme.
- Endpoint sin Policy/Gate cuando modifica o lee datos privados.
- `company_id` recibido desde el cliente sin validación.
- Falta de ownership.
- Falta de validación.
- Upsert masivo sin chunks.
- Sync sin deduplicación.
- Transacción crítica sin rollback.
- Query crítica sin índice razonable.
- Logs con datos sensibles.
- Secretos en frontend.
- Archivo demasiado grande sin justificación.
- Falta de manejo de errores.
- Falta de estados UI.
- Falta de pruebas en flujo crítico.

---

## Referencias internas obligatorias

Antes de implementar, Antigravity debe consultar los documentos dentro de `references/` según el tipo de tarea:

- `architecture.md` para estructura general.
- `backend.md` para Laravel, services, repositories, requests, policies y jobs.
- `frontend.md` para React, hooks, modales, SWR/react-query y UI modular.
- `security.md` para revisión de vulnerabilidades.
- `database.md` para PostgreSQL, índices, queries y auditoría.
- `performance.md` para optimización.
- `testing.md` para pruebas.
- `cicd.md` para pipeline y despliegue.
- `monitoring.md` para observabilidad.
- `accessibility.md` para UI accesible.
- `scalability.md` para crecimiento.
- `internationalization.md` para Perú, moneda, fechas y validaciones locales.
- `mobile-pwa.md` para experiencia móvil y PWA.
- `disaster-recovery.md` para continuidad.
- `data-privacy.md` para privacidad y cumplimiento.

---



---

## Capa PRO v7 obligatoria

Además de las reglas enterprise anteriores, esta versión incorpora controles avanzados de gobernanza del agente, operación productiva y seguridad multiempresa. Antigravity debe aplicar esta capa cuando el cambio tenga impacto real en código, datos, despliegue, permisos, archivos, exports, colas, infraestructura o contratos API.

### Plan Before Code

Antes de modificar archivos, Antigravity debe identificar:

1. Qué entendió.
2. Qué archivos revisará.
3. Qué patrón existe actualmente.
4. Qué archivos piensa crear o modificar.
5. Qué riesgos detectó.
6. Qué pruebas espera ejecutar o recomendar.

Para cambios pequeños puede hacerlo internamente. Para cambios grandes, críticos o ambiguos debe explicarlo al usuario.

### Scope Control

Antigravity debe mantenerse dentro del alcance solicitado:

- Si el usuario pidió bugfix, no hacer refactor masivo.
- Si pidió endpoint, no rediseñar todo el módulo.
- Si pidió UI, no cambiar contratos backend salvo necesidad clara.
- Si detecta deuda técnica fuera del alcance, debe reportarla como recomendación.

### Tenant Isolation

Todo flujo multiempresa debe probar y validar que:

- Usuario empresa A no puede leer datos empresa B.
- Usuario empresa A no puede actualizar datos empresa B.
- Usuario empresa A no puede borrar datos empresa B.
- Jobs, exports, reportes y cache respetan `companyId`.
- `companyId` nunca viene del cliente como fuente de verdad.

### Feature Flags

Features críticas o de alto riesgo deben poder activarse/desactivarse por ambiente, empresa, rol o rollout progresivo. Toda feature flag debe tener owner, fecha de revisión y plan de eliminación.

### Data Classification

Todo campo sensible debe clasificarse como `Public`, `Internal`, `Confidential`, `Sensitive Personal Data`, `Financial Data` o `Authentication Secrets`. La clasificación define logs, exportación, cache, auditoría y retención.

### Export Safety

Todo export debe estar autorizado, filtrado por empresa, auditado, limitado, protegido contra CSV Injection y entregado por storage privado con URL temporal.

### File Upload Security

Toda carga de archivos debe validar tamaño, extensión, MIME real, destino privado si aplica, path traversal, metadata sensible y política de acceso.

### API Idempotency

Endpoints críticos de creación, pagos, facturación, imports, sync y operaciones irreversibles deben usar `Idempotency-Key` o mecanismo equivalente.

### Contract Testing

Todo contrato API usado por frontend o terceros debe documentarse y validarse. Breaking changes requieren versión nueva, deprecación o plan de migración.

### Dependency Governance

No se deben instalar dependencias sin justificar necesidad, mantenimiento, licencia, vulnerabilidades, peso y alternativas existentes.

### Environment Parity e Infrastructure

Local, staging y producción deben mantener versiones documentadas, `.env.example` completo, datos seguros, workers, scheduler, Redis, PostgreSQL, backups, health checks y configuración reproducible.

### Queue Reliability

Jobs críticos deben ser idempotentes, tener backoff, timeout, logs, locks si aplica, dead letter handling, reprocesamiento seguro y alertas.

### Audit Trail

Acciones sensibles deben auditar actor, empresa, acción, entidad, before/after seguro, IP, user agent, request_id y timestamp. Nunca auditar secretos.

### Deletion Policy

No borrar físicamente entidades legales, financieras o auditables sin política explícita. Usar soft delete, anulación, desactivación o hard delete según tipo de dato.

### Error Taxonomy

APIs deben devolver errores consistentes con `success`, `code`, `message`, `errors` y `request_id`, sin filtrar información interna.

### Documentation

Toda feature importante debe actualizar OpenAPI, README de módulo, variables de entorno, jobs, permisos, eventos, migraciones o runbooks según aplique.

### UX Failure States

La UI debe contemplar loading, error, empty, sin permisos, offline, sesión expirada, datos parciales, retry, conflicto, mantenimiento y acciones irreversibles.

### Business Rules Registry

Reglas críticas deben documentarse para evitar duplicación o contradicción. Ejemplos: `companyId` nunca desde cliente, upserts en chunks de 200, sync idempotente, exports auditados.

### Definition of Done

Una tarea solo está terminada si compila, respeta arquitectura, valida seguridad, valida ownership, maneja errores, no expone datos, considera performance, tiene pruebas o estrategia de pruebas, documentación cuando aplique y rollback si afecta producción.

---

## Referencias PRO v7 adicionales

Consultar estos archivos según el tipo de cambio:

- `plan-before-code.md` para planificar antes de modificar.
- `scope-control.md` para evitar cambios fuera de alcance.
- `tenant-isolation.md` para aislamiento multiempresa.
- `feature-flags.md` para rollout controlado.
- `data-classification.md` para sensibilidad de datos.
- `export-safety.md` para exportaciones seguras.
- `file-upload-security.md` para cargas de archivos.
- `idempotency.md` para operaciones críticas repetibles.
- `contract-testing.md` para contratos API.
- `dependency-governance.md` para librerías.
- `environment-parity.md` para consistencia de ambientes.
- `infrastructure.md` para despliegue e infraestructura.
- `queue-reliability.md` para colas confiables.
- `audit-trail.md` para trazabilidad.
- `deletion-policy.md` para borrado, anulación y retención.
- `error-taxonomy.md` para errores API.
- `documentation.md` para documentación viva.
- `ux-failure-states.md` para estados UI avanzados.
- `business-rules-registry.md` para reglas globales.
- `definition-of-done.md` para cierre de tareas.

## Principio final

Antigravity debe construir como si cada cambio fuera a producción real, con usuarios reales, datos reales, riesgos reales y mantenimiento futuro. La prioridad no es solo que el código funcione, sino que sea seguro, comprensible, auditable, escalable y resistente.


---

# Extensión Enterprise v6.0

Esta versión incorpora un sistema operativo de ingeniería para agentes de desarrollo autónomos. Además de las reglas base de arquitectura, seguridad y performance, Antigravity debe aplicar políticas de ejecución, decision trees, anti-hallucination, migration safety, API standards, OpenAPI, observabilidad avanzada, DDD-lite, gates de producción y contratos de prompt.

## Principios v6 obligatorios

1. **Leer antes de escribir:** nunca crear archivos, endpoints, columnas, relaciones, permisos o nombres de clases sin inspeccionar el patrón existente cuando el código esté disponible.
2. **Cambio mínimo seguro:** para bugfixes, modificar solo lo necesario salvo que el usuario pida refactor explícito.
3. **Producción como estándar:** todo cambio se evalúa como si fuera a producción con usuarios, datos, auditoría y rollback.
4. **Contrato antes que implementación:** en APIs, definir request, response, error schema, autorización, ownership, paginación e idempotencia antes de codificar.
5. **Migraciones seguras:** no romper compatibilidad, evitar locks largos, usar expand/contract cuando aplique y documentar rollback.
6. **Observabilidad por diseño:** endpoints críticos, jobs, syncs y pagos deben tener correlation ID, logs estructurados, métricas y trazabilidad.
7. **No inventar contexto:** si falta una columna, permiso, relación o endpoint, Antigravity debe buscarlo en el proyecto; si no existe, declararlo como supuesto antes de implementarlo.
8. **Production Gates:** ningún cambio crítico se entrega sin revisar seguridad, performance, tests, observabilidad, migraciones y rollback.

## Decision Tree principal

Antes de implementar, clasificar el cambio:

- **Auth / permisos / usuarios:** leer `security.md`, `backend.md`, `data-privacy.md`, `production-readiness-gates.md`.
- **Sync / import / upsert:** leer `backend.md`, `database.md`, `performance.md`, `event-driven.md`, `production-readiness-gates.md`.
- **Migraciones / cambios DB:** leer `database.md`, `migration-safety.md`, `disaster-recovery.md`.
- **Endpoint API:** leer `api-standards.md`, `openapi.md`, `backend.md`, `security.md`, `testing.md`.
- **Frontend UI:** leer `frontend.md`, `design-system.md`, `accessibility.md`, `performance.md`.
- **Release / deploy:** leer `cicd.md`, `git-strategy.md`, `monitoring.md`, `disaster-recovery.md`, `production-readiness-gates.md`.
- **Arquitectura o refactor:** leer `architecture.md`, `ddd-lite.md`, `architecture-review-mode.md`, `ai-coding-constraints.md`.

## Nuevas referencias v6

Antigravity debe consultar estas referencias adicionales cuando aplique:

- `execution-policies.md`: límites de comportamiento y reglas anti-desastre.
- `decision-trees.md`: árboles de decisión por tipo de tarea.
- `ddd-lite.md`: diseño por dominios, use cases, value objects y domain events.
- `anti-hallucination.md`: reglas para no inventar código ni contexto.
- `migration-safety.md`: migraciones seguras para PostgreSQL y Laravel.
- `api-standards.md`: estándar API REST/JSON, errores, paginación e idempotencia.
- `observability-engineering.md`: OpenTelemetry, correlation IDs, SLI/SLO, tracing y métricas.
- `ai-coding-constraints.md`: restricciones específicas para agentes de IA programando.
- `architecture-review-mode.md`: modo auditor de arquitectura.
- `production-readiness-gates.md`: gates obligatorios antes de producción.
- `prompt-contracts.md`: contratos operativos para prompts frecuentes.
- `openapi.md`: documentación OpenAPI 3.1 y contract testing.
- `event-driven.md`: domain events, outbox, inbox, sagas e integraciones asíncronas.
- `design-system.md`: reglas de diseño frontend, tokens, formularios, tablas y modales.
- `git-strategy.md`: estrategia Git, commits, PRs, releases, hotfixes y changelog.

## Production Gate resumido

Antes de entregar cambios críticos, confirmar:

- [ ] Seguridad revisada.
- [ ] Ownership validado.
- [ ] Autorización validada.
- [ ] Validación de entrada aplicada.
- [ ] Performance revisada.
- [ ] Tests considerados o implementados.
- [ ] Migraciones seguras si aplica.
- [ ] Logs no exponen datos sensibles.
- [ ] Observabilidad mínima agregada.
- [ ] Rollback viable.
- [ ] Contratos API documentados si aplica.
- [ ] No se inventó contexto inexistente.

## Regla final v6

Si existe conflicto entre velocidad y seguridad, gana seguridad. Si existe conflicto entre comodidad y mantenibilidad, gana mantenibilidad. Si existe conflicto entre asumir y verificar, gana verificar.
