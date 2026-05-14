# Production Readiness Gates

## Propósito

Este documento define gates mínimos antes de declarar un cambio listo para producción.

---

## Gate 1: Functional correctness

- [ ] Requerimiento entendido.
- [ ] Casos principales cubiertos.
- [ ] Errores esperados manejados.
- [ ] No hay cambios fuera de alcance.

---

## Gate 2: Security

- [ ] Auth requerida.
- [ ] Authorization aplicada.
- [ ] Ownership validado.
- [ ] Entrada validada.
- [ ] IDOR prevenido.
- [ ] Mass Assignment prevenido.
- [ ] SQL Injection prevenido.
- [ ] XSS revisado.
- [ ] Secrets protegidos.
- [ ] Logs seguros.

---

## Gate 3: Performance

- [ ] Paginación.
- [ ] No N+1.
- [ ] Índices considerados.
- [ ] Jobs para cargas pesadas.
- [ ] Chunks de 200.
- [ ] Cache segura si aplica.
- [ ] Frontend render performance revisado.

---

## Gate 4: Database/Migrations

- [ ] Migraciones backward compatible.
- [ ] No migraciones viejas modificadas.
- [ ] Rollback viable.
- [ ] Backfill seguro si aplica.
- [ ] Índices grandes considerados.
- [ ] Backup recomendado si es crítico.

---

## Gate 5: Testing

- [ ] Unit tests.
- [ ] Feature tests.
- [ ] Authorization tests.
- [ ] Ownership tests.
- [ ] Regression tests.
- [ ] E2E si flujo crítico.

---

## Gate 6: Observability

- [ ] Logs estructurados.
- [ ] Correlation ID.
- [ ] Métricas si aplica.
- [ ] Errores enviados a Sentry.
- [ ] Jobs monitoreables.
- [ ] Alertas si es crítico.

---

## Gate 7: Release/Rollback

- [ ] CI pasa.
- [ ] Variables revisadas.
- [ ] Deploy plan.
- [ ] Rollback plan.
- [ ] Feature flag si aplica.
- [ ] Health checks.

---

## Resultado

Clasificar entrega:

- `READY_FOR_PRODUCTION`.
- `READY_FOR_STAGING`.
- `NEEDS_SECURITY_FIX`.
- `NEEDS_PERFORMANCE_REVIEW`.
- `NEEDS_TESTS`.
- `BLOCKED_BY_CONTEXT`.
