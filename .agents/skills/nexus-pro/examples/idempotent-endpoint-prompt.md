# Prompt de endpoint idempotente para Antigravity

Actúa usando el skill `nexus-pro`.

Necesito implementar un endpoint crítico idempotente:

```text
[DESCRIBIR ENDPOINT: pagos, órdenes, facturas, sync, import, etc.]
```

Requisitos:

- Usar `Idempotency-Key`.
- Asociar key a user_id, company_id, route y payload_hash.
- Repetir misma key + mismo payload debe devolver misma respuesta.
- Misma key + payload diferente debe devolver 409.
- Lock para evitar carreras.
- Expiración de keys.
- Tests de reintento.
- No duplicar datos.

Entrega:

- Diseño.
- Migración si aplica.
- Middleware/service.
- Tests.
- Checklist de idempotencia.
