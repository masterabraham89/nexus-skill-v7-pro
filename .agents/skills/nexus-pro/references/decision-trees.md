# Decision Trees

## Propósito

Este documento obliga a Antigravity a clasificar la tarea antes de programar. Cada tipo de tarea activa referencias, validaciones y gates diferentes.

---

## Árbol general

```text
¿El cambio toca datos privados?
  Sí -> security.md + data-privacy.md + ownership obligatorio
  No -> continuar

¿El cambio toca base de datos?
  Sí -> database.md + migration-safety.md
  No -> continuar

¿El cambio crea o modifica API?
  Sí -> api-standards.md + openapi.md + testing.md
  No -> continuar

¿El cambio afecta UI?
  Sí -> frontend.md + accessibility.md + design-system.md
  No -> continuar

¿El cambio es crítico para producción?
  Sí -> production-readiness-gates.md + monitoring.md + disaster-recovery.md
  No -> checklist normal
```

---

## Si el usuario pide crear endpoint

Aplicar:

1. Revisar rutas existentes.
2. Identificar dominio.
3. Definir contrato request/response.
4. Crear FormRequest.
5. Crear Policy/Gate.
6. Crear Controller delgado.
7. Crear Service.
8. Crear Repository.
9. Crear Resource.
10. Agregar test feature.
11. Actualizar OpenAPI si aplica.
12. Revisar seguridad.

No entregar endpoint sin ownership si maneja datos multiempresa.

---

## Si el usuario pide refactor

Aplicar:

1. Mapear responsabilidades actuales.
2. Detectar fat controllers, god services, components gigantes.
3. Mantener comportamiento externo.
4. Separar capas.
5. No cambiar contratos sin permiso.
6. Agregar pruebas de regresión.
7. Entregar diff conceptual.

---

## Si el usuario pide bugfix

Aplicar:

1. Reproducir flujo.
2. Aislar causa raíz.
3. Cambiar mínimo necesario.
4. Revisar seguridad colateral.
5. Crear test de regresión.
6. No hacer refactor amplio salvo que sea inevitable.

---

## Si el usuario pide sync/import

Aplicar:

1. Validar fuente de datos.
2. Normalizar payload.
3. Deducir empresa desde contexto seguro.
4. Deduplicar.
5. Procesar en chunks de 200.
6. Usar upsert.
7. Usar transaction por lote cuando aplique.
8. Usar lock si hay concurrencia.
9. Registrar auditoría/resumen.
10. Diseñar reintentos idempotentes.

---

## Si el usuario pide reporte/dashboard

Aplicar:

1. Identificar filtros.
2. Validar ownership.
3. Revisar índices.
4. Evitar `SELECT *`.
5. Paginación o agregación.
6. Materialized view si es pesado.
7. Cache con TTL si es seguro.
8. Frontend con loading/error/empty.
9. Export como job si es grande.

---

## Si el usuario pide migración

Aplicar:

1. Determinar si es destructiva.
2. Usar expand/contract si aplica.
3. Evitar locks largos.
4. Crear índices concurrentes cuando sea necesario.
5. Backfill por lotes.
6. Probar rollback.
7. Considerar feature flag.
8. Revisar backups.

---

## Si el usuario pide UI/modal/formulario

Aplicar:

1. Definir types.
2. Definir service si hay API.
3. Crear hook sin JSX.
4. Crear componentes puros.
5. Crear modal independiente.
6. Validar accesibilidad.
7. Manejar loading/error/empty/success.
8. Revisar responsive.

---

## Si el usuario pide release

Aplicar:

1. Revisar branch.
2. Ejecutar lint/typecheck/tests.
3. Revisar migraciones.
4. Revisar variables de entorno.
5. Revisar secrets.
6. Revisar health checks.
7. Revisar rollback.
8. Revisar monitoreo.
9. Preparar checklist de producción.
