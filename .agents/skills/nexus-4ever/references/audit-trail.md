# Audit Trail Reference

## Propósito

Este documento define la auditoría enterprise para acciones sensibles. La auditoría permite trazabilidad, investigación de incidentes, cumplimiento y responsabilidad operativa.

## Eventos que deben auditarse

- Login/logout relevantes.
- Cambios de roles.
- Cambios de permisos.
- Cambios de datos fiscales.
- Cambios financieros.
- Eliminaciones.
- Anulaciones.
- Exports.
- Imports.
- Sync.
- Cambios de configuración.
- Accesos denegados sospechosos.
- Impersonation si existe.

## Esquema sugerido

```sql
CREATE TABLE audit_events (
    id BIGSERIAL PRIMARY KEY,
    company_id BIGINT NULL,
    actor_id BIGINT NULL,
    action VARCHAR(120) NOT NULL,
    entity_type VARCHAR(120) NULL,
    entity_id VARCHAR(120) NULL,
    before_data JSONB NULL,
    after_data JSONB NULL,
    metadata JSONB NULL,
    ip_address VARCHAR(64) NULL,
    user_agent TEXT NULL,
    request_id VARCHAR(120) NULL,
    created_at TIMESTAMP NOT NULL
);
```

## Reglas

- No auditar secretos.
- Enmascarar datos sensibles.
- Registrar company_id.
- Registrar actor.
- Registrar request_id.
- Registrar before/after solo cuando sea seguro.
- Mantener retención definida.
- Proteger auditoría contra modificación no autorizada.

## Acciones recomendadas

Formato:

```text
module.entity.action
users.role.updated
orders.export.created
billing.invoice.voided
sync.products.completed
```

## Auditoría y transacciones

Cuando la auditoría forma parte de operación crítica, escribir dentro de la misma transacción.

Cuando la auditoría es informativa y no debe bloquear, usar evento/job pero asegurar persistencia razonable.

## Checklist

- [ ] Acción sensible identificada.
- [ ] Audit event creado.
- [ ] company_id incluido.
- [ ] actor_id incluido.
- [ ] request_id incluido.
- [ ] before/after seguro.
- [ ] Sin secretos.
- [ ] Retención definida.
- [ ] Tests considerados.
