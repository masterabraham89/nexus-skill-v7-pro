# Observability Engineering Reference

## Propósito

Este documento expande monitoreo hacia observabilidad real: logs, métricas, trazas, correlation IDs, SLI/SLO, error budgets y señales de negocio.

---

## Tres pilares

1. Logs estructurados.
2. Métricas.
3. Trazas distribuidas.

Deben conectarse con correlation ID.

---

## Correlation IDs

Toda request debe tener un identificador:

- `X-Request-ID`.
- Generado si no existe.
- Propagado a jobs.
- Propagado a integraciones externas.
- Incluido en logs y errores.

---

## OpenTelemetry

Considerar OpenTelemetry para:

- Trazas HTTP.
- Queries DB.
- Jobs.
- Redis.
- Integraciones externas.
- Latencia por operación.

---

## Structured logs JSON

Ejemplo:

```json
{
  "level": "info",
  "message": "order.created",
  "request_id": "req_123",
  "user_id": 10,
  "company_id": 3,
  "order_id": 88,
  "duration_ms": 142
}
```

No incluir secretos.

---

## SLI

Indicadores:

- Availability.
- Error rate.
- Latency p95/p99.
- Job success rate.
- Queue delay.
- DB query latency.
- Sync freshness.
- API throughput.

---

## SLO

Ejemplos:

- API disponibilidad mensual >= 99.9%.
- p95 de endpoints críticos < 500 ms.
- Jobs críticos completan en < 5 min.
- Error rate 5xx < 0.5%.

---

## Error budget

Si se consume el presupuesto de error:

- Congelar releases no críticos.
- Priorizar estabilidad.
- Revisar incidentes.
- Mejorar tests/monitoring.

---

## Business metrics

Además de métricas técnicas:

- Órdenes creadas.
- Facturas emitidas.
- Syncs completados.
- Usuarios activos.
- Errores de validación frecuentes.
- Exportaciones generadas.
- Intentos fallidos de login.

---

## Alertas accionables

Una alerta debe tener:

- Qué pasó.
- Severidad.
- Servicio afectado.
- Link a dashboard/logs.
- Runbook.
- Umbral claro.

Evitar alertas ruidosas.

---

## Checklist observability

- [ ] Correlation ID.
- [ ] Logs estructurados.
- [ ] Métricas técnicas.
- [ ] Métricas de negocio.
- [ ] Tracing considerado.
- [ ] SLI/SLO definidos si aplica.
- [ ] Alertas accionables.
- [ ] Runbook vinculado.
