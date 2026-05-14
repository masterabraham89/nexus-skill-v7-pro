# Queue Reliability Reference

## Propósito

Este documento define prácticas para colas y jobs confiables. Los jobs deben tolerar reintentos, fallos parciales, concurrencia y reprocesamiento sin duplicar ni corromper datos.

## Colas recomendadas

```text
critical
default
exports
imports
notifications
low
```

## Reglas de jobs

- Idempotentes.
- Retry con backoff.
- Timeout explícito.
- Logs con request/batch/company.
- Locks cuando exista concurrencia.
- Chunks de 200 en procesos masivos.
- No cargar datasets gigantes.
- No depender de estado mutable no persistido.

## Dead Letter Queue

Todo job fallido definitivamente debe quedar inspeccionable.

Registrar:

- job;
- payload seguro;
- error;
- intentos;
- company_id;
- user_id si aplica;
- fecha;
- stack trace seguro.

## Reprocesamiento seguro

Antes de reprocesar:

- verificar idempotencia;
- verificar duplicados;
- verificar estado actual;
- reprocesar por lote;
- auditar acción.

## Unique jobs

Usar unique jobs para evitar duplicados:

- sync por empresa;
- export por usuario/filtro;
- import por archivo;
- generación de reporte.

## Backoff

Ejemplo:

```php
public function backoff(): array
{
    return [60, 300, 900];
}
```

## Alertas

Alertar por:

- jobs fallidos;
- cola acumulada;
- workers caídos;
- tiempo de espera alto;
- retries excesivos;
- deadlocks.

## Checklist

- [ ] Job idempotente.
- [ ] Backoff configurado.
- [ ] Timeout definido.
- [ ] Cola correcta.
- [ ] Lock si aplica.
- [ ] Chunks de 200.
- [ ] Logs seguros.
- [ ] Dead letter inspeccionable.
- [ ] Reprocesamiento seguro.
- [ ] Alertas configuradas.
