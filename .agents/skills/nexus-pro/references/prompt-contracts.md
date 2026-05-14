# Prompt Contracts

## Propósito

Los prompt contracts indican qué debe hacer Antigravity automáticamente según la intención del usuario.

---

## Contrato: crear endpoint

Debe producir o modificar:

- Route.
- Controller.
- FormRequest.
- Policy/Gate.
- Service.
- Repository.
- Resource.
- Test feature.
- OpenAPI schema.
- Logs/correlation si aplica.

Debe validar:

- Auth.
- Ownership.
- Validation.
- Rate limit.
- Error schema.

---

## Contrato: crear módulo frontend

Debe producir:

- Page.
- Components.
- Hook.
- Modal si aplica.
- Service.
- Types.
- Tests si aplica.

Debe validar:

- No fetch directo en components.
- Hooks sin JSX.
- Loading/error/empty.
- Accesibilidad.
- Responsive.

---

## Contrato: corregir bug

Debe entregar:

- Causa raíz.
- Cambio mínimo.
- Archivos modificados.
- Prueba de regresión.
- Riesgos.

No debe:

- Refactorizar sin necesidad.
- Cambiar contratos.
- Crear dependencias.

---

## Contrato: revisar seguridad

Debe entregar:

- Hallazgos por severidad.
- Evidencia.
- Impacto.
- Recomendación.
- Fix propuesto.
- Checklist OWASP.

---

## Contrato: crear migración

Debe entregar:

- Migración.
- Riesgo de lock.
- Rollback.
- Backfill si aplica.
- Plan expand/contract si aplica.
- Prueba en staging recomendada.

---

## Contrato: preparar release

Debe entregar:

- Checklist CI/CD.
- Migraciones.
- Variables.
- Tests.
- Health checks.
- Rollback.
- Riesgos.
- Monitoreo.
