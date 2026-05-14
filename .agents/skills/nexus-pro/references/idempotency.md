# Idempotency Reference

## Propósito

Este documento define reglas para operaciones idempotentes. La idempotencia evita duplicados cuando el cliente reintenta, la red falla, un job se ejecuta dos veces o una integración externa responde tarde.

## Cuándo usar Idempotency-Key

Obligatorio en endpoints críticos:

- pagos;
- órdenes;
- facturas;
- imports;
- sync;
- creación masiva;
- reservas;
- operaciones financieras;
- cambios de estado irreversibles.

## Header recomendado

```http
Idempotency-Key: 01HXAMPLEKEY123456789
```

## Reglas backend

- La key debe asociarse a usuario, empresa, endpoint y payload hash.
- Repetir la misma key con mismo payload devuelve la misma respuesta.
- Repetir la misma key con payload distinto debe devolver conflicto 409.
- Las keys deben expirar.
- El procesamiento debe usar lock para evitar carrera.

## Tabla sugerida

```sql
CREATE TABLE idempotency_keys (
    id BIGSERIAL PRIMARY KEY,
    company_id BIGINT NOT NULL,
    user_id BIGINT NOT NULL,
    key VARCHAR(120) NOT NULL,
    route VARCHAR(180) NOT NULL,
    payload_hash VARCHAR(128) NOT NULL,
    response_status INTEGER,
    response_body JSONB,
    locked_until TIMESTAMP NULL,
    created_at TIMESTAMP NOT NULL,
    expires_at TIMESTAMP NOT NULL,
    UNIQUE (company_id, user_id, key, route)
);
```

## Jobs

Los jobs deben ser idempotentes mediante:

- unique job IDs;
- locks;
- external IDs;
- upsert;
- estado procesado;
- deduplicación por empresa.

## Sync

Toda sincronización debe:

- deduplicar por `company_id + external_id`;
- usar upsert;
- procesar en chunks de 200;
- poder reejecutarse sin duplicar;
- registrar lote y resultado.

## Checklist

- [ ] Endpoint crítico identificado.
- [ ] Idempotency-Key exigida.
- [ ] Payload hash calculado.
- [ ] Lock aplicado.
- [ ] Respuesta persistida.
- [ ] Conflicto 409 en payload distinto.
- [ ] Expiración definida.
- [ ] Tests de reintento.
