<div align="center">

# ⚡ NEXUS-PRO

### *Neural Expert for Unified eXtended Systems — Enterprise Edition*

**El skill de ingeniería enterprise para Antigravity más completo del ecosistema.**  
Arquitectura limpia · Seguridad por defecto · Preparado para producción real.

[![Version](https://img.shields.io/badge/version-7.0--pro--enterprise-blue?style=for-the-badge)](./CHANGELOG.md)
[![Platform](https://img.shields.io/badge/platform-Antigravity-purple?style=for-the-badge)](#)
[![Category](https://img.shields.io/badge/category-Enterprise%20Engineering-orange?style=for-the-badge)](#)
[![Stack](https://img.shields.io/badge/stack-Laravel%20%7C%20React%20%7C%20PostgreSQL-green?style=for-the-badge)](#)

</div>

---

## ¿Qué es NEXUS-PRO?

NEXUS-PRO es un **skill enterprise para Antigravity** que convierte al agente en un arquitecto de software senior especializado en proyectos full stack con Laravel, React, TypeScript y PostgreSQL.

En lugar de generar código genérico, con NEXUS-PRO Antigravity produce código que sigue **estándares reales de producción**: separación de responsabilidades, seguridad por defecto, aislamiento multiempresa, validación robusta, manejo de errores, observabilidad y preparación para escalar.

> **Sin NEXUS-PRO**, Antigravity puede mezclar lógica de negocio en controladores, olvidar validar ownership, generar código sin transacciones, o crear componentes React con fetch directo.  
> **Con NEXUS-PRO**, cada entrega sigue la misma arquitectura, seguridad y estándar de calidad que un equipo enterprise senior.

---

## ¿Para quién es este skill?

✅ Desarrolladores que construyen **plataformas SaaS multi-tenant**  
✅ Equipos que trabajan en **sistemas POS** con sincronización offline/online  
✅ Proyectos de **e-commerce** con backend Laravel y panel de administración  
✅ Aplicaciones con **roles, permisos y auditoría financiera**  
✅ APIs REST con **frontend React/TypeScript modular**  
✅ Apps **PWA o Capacitor** que requieren rendimiento móvil  
✅ Cualquier proyecto que necesite **arquitectura limpia y código mantenible**

---

## Qué incluye

```
nexus-skill-v7-pro-enterprise/
└── .agents/skills/nexus-pro/
    ├── SKILL.md              ← Núcleo del skill (645 líneas de reglas enterprise)
    ├── MANIFEST.md           ← Índice de todos los archivos
    ├── CHANGELOG.md          ← Historial de versiones
    ├── examples/             ← 13 prompts listos para usar
    ├── references/           ← 50 documentos técnicos especializados
    └── templates/            ← 15 plantillas de código operativas
```

### 50 Referencias técnicas cubren:

| Área | Documentos |
|------|-----------|
| **Arquitectura** | architecture, ddd-lite, decision-trees, api-standards |
| **Backend Laravel** | backend, security, database, migration-safety, queue-reliability |
| **Frontend React** | frontend, design-system, mobile-pwa, ux-failure-states |
| **Seguridad** | security, tenant-isolation, data-privacy, data-classification, anti-hallucination |
| **Operaciones** | cicd, monitoring, observability-engineering, disaster-recovery, infrastructure |
| **Gobierno** | execution-policies, scope-control, audit-trail, definition-of-done, feature-flags |
| **Calidad** | testing, performance, scalability, accessibility, dependency-governance |

### 13 Prompts listos para usar:

- `bugfix-prompt.md` — debug de endpoints y sincronizaciones
- `create-endpoint-prompt.md` — nuevo endpoint con seguridad completa
- `new-feature-prompt.md` — funcionalidad full stack enterprise
- `refactor-prompt.md` — refactorización sin romper contratos
- `security-review-prompt.md` — auditoría de seguridad
- `tenant-isolation-review-prompt.md` — revisión de aislamiento multiempresa
- `idempotent-endpoint-prompt.md` — endpoints idempotentes para pagos/imports
- `migration-safe-prompt.md` — migraciones sin downtime
- `export-feature-prompt.md` — exports seguros y auditados
- `file-upload-feature-prompt.md` — carga de archivos con validación completa
- `dependency-review-prompt.md` — revisión de dependencias
- `architecture-review-prompt.md` — auditoría de arquitectura
- `production-release-prompt.md` — checklist de release a producción

### 15 Plantillas operativas:

- `frontend-module-template.md` — módulo React completo con hooks, services y types
- `backend-service-template.md` — Service Laravel con transacciones y auditoría
- `code-review-checklist.md` — checklist técnico de revisión de código
- `api-endpoint-template.md` — endpoint con FormRequest, Policy y Resource
- `migration-template.md` — migración segura con rollback
- `audit-event-template.md` — evento de auditoría estructurado
- `export-job-template.md` — job de exportación seguro
- Y 8 más...

---

## Instalación

### Opción A — Manual

1. Descarga o clona este repositorio:

```bash
git clone https://github.com/tu-usuario/nexus-skill-v7-pro-enterprise.git
```

2. Copia la carpeta `.agents` dentro de la raíz de tu proyecto:

```bash
cp -r nexus-skill-v7-pro-enterprise/.agents tu-proyecto/.agents
```

3. Antigravity detectará automáticamente el skill al iniciar una sesión dentro de ese proyecto.

### Opción B — Global (todos tus proyectos)

Coloca la carpeta `.agents` en el directorio raíz de tu workspace de Antigravity para que aplique a todos los proyectos.

---

## Cómo activar el skill

Una vez instalado, escribe en Antigravity cualquiera de estos triggers y el skill se activa automáticamente:

```
"crear endpoint"
"refactorizar backend"
"crear módulo React"
"revisar seguridad"
"optimizar performance"
"debug API"
"preparar despliegue"
"auditar código"
```

O puedes invocarlo explícitamente:

```
Actúa usando el skill nexus-skill-v7-pro-enterprise.
Necesito crear un endpoint para [tu tarea].
```

---

## Inicio rápido — 5 pasos

```
1. Instala el skill en tu proyecto (ver sección Instalación)
2. Abre Antigravity en la raíz de tu proyecto
3. Describe tu tarea con cualquiera de los triggers
4. Usa uno de los 13 prompts de examples/ como base
5. Recibe código enterprise listo para producción
```

---

## Stack soportado

| Capa | Tecnología |
|------|-----------|
| **Backend** | Laravel 10+ / PHP 8.2+ |
| **Frontend** | React 18+ / TypeScript 5+ |
| **Base de datos** | PostgreSQL 15+ |
| **Auth** | Laravel Sanctum |
| **Cache / Queue** | Redis |
| **Deploy** | Cualquier servidor o plataforma cloud |
| **Mobile** | Capacitor / PWA |

---

## Reglas de oro que aplica este skill

- **Evidence-Based Debugging** — Cero parcheo ciego. Los errores se triangulan y verifican con pruebas reales antes de escribir código.
- **Anti-Patch Policy (Refactor-First)** — Límites estrictos de tamaño. El código spaghetti está prohibido y fuerza una refactorización hacia Servicios/Hooks.
- **Controllers no tocan la base de datos** — delegan a Services y Repositories. Máximo 3 dependencias.
- **Services son stateless** — y completamente aislados de la petición HTTP.
- **companyId nunca viene del cliente** — siempre desde el token autenticado
- **Ownership siempre validado** — empresa A no puede ver datos de empresa B
- **Upserts en chunks de 200** — nunca operaciones masivas sin chunking
- **useEffect siempre con cleanup** — sin memory leaks en React. Máximo 2 efectos por componente.
- **No `any` sin justificación** — TypeScript estricto
- **Transacciones con rollback** — consistencia en operaciones críticas
- **Logs seguros** — nunca tokens, passwords o datos bancarios en logs
- **Sync idempotente** — se puede ejecutar dos veces sin duplicar datos
- **TDD Strict Mode** — escribe la prueba antes de escribir lógica crítica
- **Database Indexing** — prohibidas claves foráneas sin `$table->index()`
- **Seguridad Activa** — todo endpoint público debe tener rate limiting para prevenir DDoS
- **Conventional Commits** — historial de git estructurado obligatoriamente (`feat:`, `fix:`)
- **No-Delegation Policy** — La IA tiene prohibido delegar comandos, debe autoejecutarlos según el `nexus-workflow.md` local

---

## Comparativa rápida

| Característica | Skill genérico | NEXUS-PRO |
|----------------|---------------|-----------|
| Arquitectura Controller→Service→Repository | ❌ | ✅ Obligatoria |
| Tenant isolation multiempresa | ❌ | ✅ Validado siempre |
| Seguridad OWASP Top 10 | ❌ | ✅ Checklist activo |
| Upserts en chunks | ❌ | ✅ Regla obligatoria |
| 50 referencias técnicas | ❌ | ✅ |
| 13 prompts listos | ❌ | ✅ |
| 15 plantillas operativas | ❌ | ✅ |
| Production Gate antes de entregar | ❌ | ✅ |
| Plan Before Code | ❌ | ✅ Siempre |
| Definition of Done | ❌ | ✅ Checklist |

---

## Versiones

| Versión | Highlights |
|---------|-----------|
| **v7.0-pro-enterprise** | God Mode Debugging, Anti-Patch Policy, Plan Before Code, Tenant Isolation, Feature Flags, Idempotency, Export Safety, Definition of Done |
| **v6.0-enterprise** | DDD-lite, Migration Safety, OpenAPI, Event-Driven, Observability Engineering, Production Gates |
| **v5.0-ultimate** | Base enterprise: Laravel/React/PostgreSQL, seguridad, CI/CD, testing, PWA |

Ver [CHANGELOG.md](./.agents/skills/nexus-pro/CHANGELOG.md) para el historial completo.

---

## Contribuir

¿Encontraste un gap? ¿Tienes una referencia técnica que falta? Las contribuciones son bienvenidas.

1. Abre un issue describiendo la mejora
2. Propón el nuevo archivo en `references/` o `examples/`
3. Envía un Pull Request

Ver [CONTRIBUTING.md](./CONTRIBUTING.md) para el flujo completo.

---

## Autor

**Abraham Apeña Rosas**  
Empresa: Antigravity  
Plataforma: [Antigravity](https://antigravity.dev)

---

<div align="center">

**NEXUS-PRO** · v7.0-pro-enterprise · Enterprise Software Engineering Skill for Antigravity

*Construye como si cada cambio fuera a producción real, con usuarios reales, datos reales y mantenimiento futuro.*

</div>
