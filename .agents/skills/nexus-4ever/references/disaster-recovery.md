# Disaster Recovery Reference

## Propósito

Este documento define prácticas de recuperación ante desastres para NEXUS-4EVER. El objetivo es reducir impacto, pérdida de datos y tiempo de indisponibilidad.

---

## Conceptos clave

### RTO

Recovery Time Objective. Tiempo máximo aceptable para restaurar servicio.

Ejemplo:

```text
RTO crítico: 1 hora
RTO normal: 4 horas
```

### RPO

Recovery Point Objective. Pérdida máxima aceptable de datos.

Ejemplo:

```text
RPO crítico: 15 minutos
RPO normal: 1 hora
```

---

## Backups horarios

Para sistemas críticos:

- Backups horarios.
- Backups diarios.
- Backups semanales.
- Retención definida.
- Cifrado.
- Almacenamiento fuera del servidor principal.
- Validación automática.
- Pruebas de restauración.

---

## Restauración

Debe existir procedimiento para:

1. Identificar backup correcto.
2. Restaurar en ambiente aislado.
3. Validar integridad.
4. Validar aplicación.
5. Cambiar tráfico si aplica.
6. Confirmar operación.
7. Documentar incidente.

Nunca asumir que un backup sirve sin probar restauración.

---

## Failover

Cuando la infraestructura lo permita:

- DB replica.
- App nodes múltiples.
- Load balancer.
- CDN.
- Redis gestionado.
- Object storage redundante.

Considerar:

- DNS TTL.
- Replication lag.
- Promoción de réplica.
- Reconfiguración de app.
- Consistencia posterior.

---

## Incidentes P1-P4

### P1 crítico

- Sistema caído.
- Pérdida de datos.
- Brecha de seguridad.
- Pagos/facturación inutilizables.
- Impacto masivo.

Respuesta inmediata y escalamiento total.

### P2 alto

- Función crítica degradada.
- Errores frecuentes.
- Latencia severa.
- Jobs críticos fallando.

Respuesta prioritaria.

### P3 medio

- Bug parcial.
- Módulo secundario afectado.
- Workaround disponible.

Planificar corrección.

### P4 bajo

- Problema menor.
- UI inconsistente.
- Mejora no urgente.

Backlog controlado.

---

## Runbooks

Cada runbook debe incluir:

- Nombre del incidente.
- Síntomas.
- Alertas relacionadas.
- Diagnóstico.
- Comandos seguros.
- Pasos de mitigación.
- Pasos de rollback.
- Validaciones.
- Escalamiento.
- Contactos.
- Postmortem.

---

## Rollback de deploy

Debe estar documentado:

- Identificar versión anterior.
- Revertir build.
- Revertir feature flag.
- Revertir migración si es seguro.
- Restaurar backup si hay corrupción.
- Limpiar cache.
- Reiniciar workers.
- Validar health checks.

---

## Respuesta a caída del sistema

Pasos:

1. Confirmar caída.
2. Revisar health checks.
3. Revisar logs.
4. Revisar DB.
5. Revisar Redis.
6. Revisar workers.
7. Revisar último deploy.
8. Activar rollback si el deploy causó el problema.
9. Comunicar estado.
10. Documentar causa raíz.

---

## Postmortem

Después de incidente P1/P2:

- Qué pasó.
- Impacto.
- Línea de tiempo.
- Causa raíz.
- Qué funcionó.
- Qué falló.
- Acciones preventivas.
- Dueños y fechas.
- Actualización de runbooks.

El postmortem debe ser sin cultura de culpa y orientado a aprendizaje.

---

## Checklist disaster recovery

- [ ] RTO definido.
- [ ] RPO definido.
- [ ] Backups horarios si aplica.
- [ ] Restauración probada.
- [ ] Runbooks existentes.
- [ ] Rollback documentado.
- [ ] Health checks disponibles.
- [ ] Failover considerado.
- [ ] Incidentes clasificados P1-P4.
- [ ] Postmortem requerido para P1/P2.
