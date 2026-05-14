# Architecture Review Mode

## Propósito

Modo auditor para evaluar calidad arquitectónica de un módulo sin necesariamente modificarlo.

---

## Cuándo activarlo

- Antes de refactor grande.
- Antes de escalar un módulo.
- Cuando hay bugs recurrentes.
- Cuando un archivo creció demasiado.
- Antes de una feature compleja.
- Antes de producción.

---

## Señales de deuda técnica

Backend:

- Fat controllers.
- Services de más de 400 líneas.
- Repositories con negocio.
- Queries duplicadas.
- Falta de policies.
- Falta de FormRequest.
- Jobs no idempotentes.
- Transactions ausentes.
- Logs inseguros.

Frontend:

- Pages gigantes.
- Components con fetch directo.
- Hooks con JSX.
- Modales inline enormes.
- Props drilling excesivo.
- `any` masivo.
- Estados UI incompletos.
- Renders lentos.

DB:

- Índices faltantes.
- N+1.
- `SELECT *`.
- Tablas sin constraints.
- Migraciones peligrosas.
- Falta de auditoría.

---

## Reporte esperado

1. Resumen ejecutivo.
2. Riesgos críticos.
3. Riesgos altos.
4. Riesgos medios.
5. Riesgos bajos.
6. Mapa de responsabilidades actuales.
7. Propuesta de arquitectura objetivo.
8. Plan incremental.
9. Pruebas necesarias.
10. Riesgos de migración.

---

## Plan incremental

Separar refactor en fases:

- Fase 1: seguridad y ownership.
- Fase 2: separar controller/service/repository.
- Fase 3: tests de regresión.
- Fase 4: performance.
- Fase 5: observabilidad.

---

## Checklist revisión

- [ ] Capas separadas.
- [ ] Responsabilidades claras.
- [ ] Seguridad revisada.
- [ ] Performance revisada.
- [ ] Tests existentes identificados.
- [ ] Riesgo de regresión estimado.
- [ ] Plan incremental definido.
