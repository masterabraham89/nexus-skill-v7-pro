# Event-Driven Reference

## Propósito

Este documento define patrones asíncronos para sistemas escalables y resilientes.

---

## Domain Events

Eventos de dominio nombran hechos ocurridos:

- `OrderCreated`.
- `InvoicePaid`.
- `InventorySynced`.
- `UserPermissionChanged`.

Reglas:

- Nombre en pasado.
- Payload mínimo.
- No incluir secretos.
- Incluir IDs, no objetos gigantes.
- Procesar efectos secundarios en listeners/jobs.

---

## Outbox Pattern

Usar cuando se necesite garantizar que un evento externo se publique después de una transacción.

Flujo:

1. Guardar cambio de negocio.
2. Guardar evento en tabla outbox en la misma transacción.
3. Worker publica evento.
4. Marcar como publicado.
5. Reintentar si falla.

---

## Inbox Pattern

Usar para consumir eventos externos de forma idempotente:

1. Recibir evento.
2. Verificar event ID.
3. Si ya fue procesado, ignorar.
4. Procesar.
5. Registrar resultado.

---

## Saga Pattern

Usar en procesos distribuidos con múltiples pasos:

- Reserva inventario.
- Crea orden.
- Cobra pago.
- Emite factura.
- Envía notificación.

Cada paso debe tener compensación si falla.

---

## Event Replay

Si se soporta replay:

- Eventos inmutables.
- Versionado de eventos.
- Idempotencia.
- Control de orden.
- Ambiente de prueba.

---

## Async Integration Safety

Reglas:

- Idempotency keys.
- Retry con backoff.
- Dead-letter queue.
- Timeouts.
- Circuit breaker.
- Logs con correlation ID.
- Métricas de éxito/fallo.

---

## Checklist event-driven

- [ ] Evento nombrado en pasado.
- [ ] Payload mínimo.
- [ ] Idempotencia.
- [ ] Outbox si requiere garantía.
- [ ] Inbox si consume externo.
- [ ] Retry/backoff.
- [ ] Dead-letter.
- [ ] Observabilidad.
