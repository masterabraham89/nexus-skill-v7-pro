# Error Taxonomy Reference

## Propósito

Este documento define una taxonomía de errores estándar para APIs y frontend. Los errores deben ser consistentes, trazables, seguros y útiles.

## Formato API recomendado

```json
{
  "success": false,
  "code": "ORDER_NOT_FOUND",
  "message": "No se encontró la orden solicitada.",
  "errors": [],
  "request_id": "req_123"
}
```

## Reglas

- Todo error debe tener `code` estable.
- El mensaje debe ser entendible.
- No exponer stack traces.
- No exponer SQL.
- No revelar si un recurso existe en otra empresa.
- Incluir `request_id`.
- Validation errors deben usar estructura clara.

## Códigos por categoría

### Auth

```text
AUTH_REQUIRED
AUTH_INVALID_TOKEN
AUTH_EXPIRED_TOKEN
AUTH_FORBIDDEN
```

### Validation

```text
VALIDATION_FAILED
INVALID_DATE_RANGE
INVALID_FILE_TYPE
INVALID_AMOUNT
```

### Domain

```text
ORDER_NOT_FOUND
ORDER_ALREADY_CANCELLED
INVOICE_CANNOT_BE_DELETED
SYNC_ALREADY_RUNNING
```

### Conflict

```text
IDEMPOTENCY_CONFLICT
DUPLICATE_RECORD
STATE_CONFLICT
```

### Rate limit

```text
RATE_LIMIT_EXCEEDED
LOGIN_ATTEMPTS_EXCEEDED
```

### System

```text
INTERNAL_ERROR
SERVICE_UNAVAILABLE
EXTERNAL_PROVIDER_ERROR
```

## HTTP mapping

| HTTP | Uso |
|---|---|
| 400 | Request mal formado |
| 401 | No autenticado |
| 403 | No autorizado |
| 404 | No encontrado o no perteneciente |
| 409 | Conflicto de estado/idempotencia |
| 422 | Validación |
| 429 | Rate limit |
| 500 | Error inesperado |
| 503 | Servicio no disponible |

## Frontend

El frontend debe:

- mostrar mensaje claro;
- manejar sesión expirada;
- manejar sin permisos;
- permitir retry cuando aplique;
- no mostrar errores técnicos crudos;
- loggear request_id para soporte.

## Checklist

- [ ] Error code estable.
- [ ] Mensaje seguro.
- [ ] request_id incluido.
- [ ] HTTP correcto.
- [ ] Validation errors estructurados.
- [ ] Frontend maneja el caso.
- [ ] No se exponen internals.
