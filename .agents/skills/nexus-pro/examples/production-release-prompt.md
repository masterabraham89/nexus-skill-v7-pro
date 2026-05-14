# Prompt: revisión antes de producción

Actúa usando `production-readiness-gates.md` del skill `nexus-pro`.

Necesito revisar este cambio antes de producción:

```text
[DESCRIBIR CAMBIO / PR / RELEASE]
```

## Validar

- Seguridad.
- Performance.
- Migraciones.
- Tests.
- Observabilidad.
- CI/CD.
- Rollback.
- Health checks.
- Variables de entorno.
- Riesgos.

## Entrega

Clasifica el release como:

- READY_FOR_PRODUCTION
- READY_FOR_STAGING
- NEEDS_SECURITY_FIX
- NEEDS_PERFORMANCE_REVIEW
- NEEDS_TESTS
- BLOCKED_BY_CONTEXT

Incluye checklist completo.
