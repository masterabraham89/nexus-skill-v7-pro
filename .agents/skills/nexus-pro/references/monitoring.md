# Monitoring Reference

## Propósito

Este documento define la observabilidad mínima para NEXUS-PRO. Un sistema en producción debe permitir detectar, diagnosticar y responder a incidentes rápidamente.

---

## Herramientas recomendadas

- Sentry para errores frontend/backend.
- Laravel Telescope para debugging en entornos controlados.
- Laravel Pulse para métricas de aplicación.
- UptimeRobot o equivalente para disponibilidad.
- Web Vitals para experiencia frontend.
- Logs centralizados.
- Métricas de base de datos.
- Alertas por latencia, errores y caídas.

---

## Sentry

Usar Sentry para:

- Errores backend.
- Errores frontend.
- Excepciones no controladas.
- Trazas.
- Releases.
- Source maps.
- Contexto seguro de usuario.

No enviar:

- Passwords.
- Tokens.
- API keys.
- Datos personales innecesarios.
- Payloads sensibles completos.

---

## Laravel Telescope

Uso recomendado:

- Desarrollo.
- Staging.
- Diagnóstico temporal.

Evitar exponer Telescope en producción pública. Si se usa en producción, proteger con autorización estricta.

Monitorear:

- Requests.
- Queries.
- Jobs.
- Exceptions.
- Mail.
- Notifications.
- Cache.
- Logs.

---

## Laravel Pulse

Usar para observar:

- Performance de endpoints.
- Jobs.
- Queries lentas.
- Uso de cache.
- Errores.
- Usuarios activos.
- Latencia.

---

## UptimeRobot

Configurar checks para:

- API principal.
- Frontend.
- Endpoint `/health`.
- Login básico si existe health autenticado separado.
- Servicios críticos.

Alertas:

- Email.
- Slack/Discord/Teams si aplica.
- Escalamiento según severidad.

---

## Web Vitals

Medir:

- LCP.
- INP.
- CLS.
- TTFB.
- FCP.

Acciones:

- Optimizar imágenes.
- Reducir JavaScript.
- Mejorar cache.
- Evitar layout shifts.
- Optimizar servidor.

---

## Alertas

Debe haber alertas para:

- Caída de sistema.
- Error rate alto.
- Latencia alta.
- Jobs fallando.
- Cola acumulada.
- DB sin conexión.
- Redis sin conexión.
- Espacio en disco bajo.
- Uso de CPU/RAM crítico.
- Fallos de login anómalos.
- Errores 500 frecuentes.

---

## Logs

Los logs deben ser estructurados y buscables.

Campos recomendados:

- `timestamp`.
- `level`.
- `request_id`.
- `user_id`.
- `company_id`.
- `action`.
- `entity`.
- `duration_ms`.
- `status_code`.
- `error_code`.

No registrar secretos ni PII innecesaria.

---

## Métricas de error

Medir:

- Errores 4xx.
- Errores 5xx.
- Excepciones por endpoint.
- Fallos de jobs.
- Fallos de integraciones.
- Fallos de validación inusuales.
- Fallos de autorización.

---

## Métricas de latencia

Medir:

- p50.
- p95.
- p99.
- Tiempo por endpoint.
- Tiempo por query.
- Tiempo por job.
- Tiempo de integraciones externas.

---

## Monitoreo DB

Medir:

- Queries lentas.
- Locks.
- Deadlocks.
- Uso de conexiones.
- Tamaño de tablas.
- Tamaño de índices.
- Cache hit ratio.
- Replication lag si hay réplicas.
- Vacuum/analyze.
- Crecimiento anómalo.

---

## Incidentes

Cada incidente debe registrar:

- Hora de inicio.
- Hora de detección.
- Hora de mitigación.
- Hora de resolución.
- Impacto.
- Causa raíz.
- Acciones tomadas.
- Acciones preventivas.
- Responsable.

---

## Checklist monitoring

- [ ] Sentry configurado.
- [ ] Logs estructurados.
- [ ] Health check disponible.
- [ ] Uptime configurado.
- [ ] Métricas DB visibles.
- [ ] Jobs monitoreados.
- [ ] Alertas configuradas.
- [ ] Web Vitals considerados.
- [ ] Runbook de incidentes disponible.
