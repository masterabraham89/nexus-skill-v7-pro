# Production Readiness Checklist

## Estado

- Release:
- Fecha:
- Responsable:
- Clasificación:

## Gates

### Functional

- [ ] Requerimiento completo.
- [ ] Casos borde revisados.

### Security

- [ ] Auth.
- [ ] Authorization.
- [ ] Ownership.
- [ ] Validation.
- [ ] Logs seguros.

### Performance

- [ ] Paginación.
- [ ] Índices.
- [ ] No N+1.
- [ ] Jobs/chunks.

### Database

- [ ] Migraciones seguras.
- [ ] Rollback.
- [ ] Backup.

### Testing

- [ ] Unit.
- [ ] Feature.
- [ ] E2E.
- [ ] Regression.

### Observability

- [ ] Logs.
- [ ] Metrics.
- [ ] Traces.
- [ ] Alerts.

### Release

- [ ] CI pasa.
- [ ] Variables.
- [ ] Health checks.
- [ ] Rollback.

## Resultado

```text
READY_FOR_PRODUCTION | READY_FOR_STAGING | NEEDS_FIX | BLOCKED
```
