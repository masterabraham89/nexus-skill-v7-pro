# API Standards Reference

## Propósito

Este documento define el estándar API para NEXUS-4EVER. Toda API debe ser consistente, segura, versionada y documentable.

---

## Versionado

Usar rutas versionadas:

```text
/api/v1/orders
/api/v1/customers
```

No romper contratos dentro de la misma versión sin estrategia backward compatible.

---

## Formato JSON estándar

Respuesta exitosa:

```json
{
  "success": true,
  "message": "Operación completada.",
  "data": {},
  "meta": {}
}
```

Respuesta de error:

```json
{
  "success": false,
  "message": "No se pudo procesar la solicitud.",
  "error": {
    "code": "VALIDATION_ERROR",
    "details": {}
  }
}
```

Si el proyecto ya tiene estándar distinto, mantenerlo y documentarlo.

---

## Códigos HTTP

- 200 OK: lectura o actualización exitosa.
- 201 Created: creación exitosa.
- 204 No Content: eliminación exitosa sin body.
- 400 Bad Request: request inválido.
- 401 Unauthorized: no autenticado.
- 403 Forbidden: autenticado sin permiso.
- 404 Not Found: no existe o no pertenece.
- 409 Conflict: conflicto de negocio.
- 422 Unprocessable Entity: validación.
- 429 Too Many Requests: rate limit.
- 500 Internal Server Error: error inesperado.

---

## Paginación

Usar paginación en listados.

Parámetros:

```text
?page=1&per_page=25
```

Límites:

- Default: 25.
- Máximo: 100 salvo justificación.

Meta:

```json
{
  "meta": {
    "current_page": 1,
    "per_page": 25,
    "total": 240,
    "last_page": 10
  }
}
```

Para alto volumen, preferir cursor pagination.

---

## Filtros y sorting

Usar whitelist:

```php
$allowedSorts = ['created_at', 'name', 'status'];
```

Nunca usar columnas recibidas directamente sin validar.

---

## Idempotency keys

Para operaciones sensibles:

- Pagos.
- Órdenes.
- Imports.
- Sync.
- Webhooks.

Usar header:

```text
Idempotency-Key: uuid
```

Guardar resultado asociado a usuario/empresa/key.

---

## Correlation ID

Toda request debe tener correlation ID:

- Recibir `X-Request-ID` si viene.
- Generarlo si no viene.
- Incluirlo en logs.
- Devolverlo en response header.

---

## Errores de dominio

Crear códigos claros:

- `ORDER_ALREADY_APPROVED`.
- `INSUFFICIENT_PERMISSIONS`.
- `RESOURCE_NOT_OWNED`.
- `SYNC_ALREADY_RUNNING`.
- `VALIDATION_ERROR`.

No devolver trazas internas.

---

## Seguridad API

- Auth obligatoria en datos privados.
- Ownership por empresa.
- Rate limits.
- Payload size limit.
- Validación estricta.
- No exponer campos sensibles.
- No confiar en frontend.

---

## Checklist API

- [ ] Ruta versionada.
- [ ] FormRequest.
- [ ] Policy/Gate.
- [ ] Resource.
- [ ] Paginación.
- [ ] Error schema.
- [ ] OpenAPI actualizado.
- [ ] Correlation ID.
- [ ] Idempotency si aplica.
- [ ] Rate limit si aplica.
