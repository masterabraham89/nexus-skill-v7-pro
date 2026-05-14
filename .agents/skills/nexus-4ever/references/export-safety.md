# Export Safety Reference

## Propósito

Este documento define reglas para exportaciones seguras. Los exports son una fuente frecuente de fugas de datos, mezcla de tenants, archivos públicos y CSV Injection.

## Regla principal

Todo export debe ser autorizado, filtrado por empresa, auditado, limitado y entregado de forma privada.

## Riesgos

- Exportar datos de otra empresa.
- Exportar más columnas de las necesarias.
- Archivos públicos permanentes.
- CSV Injection.
- Exports pesados bloqueando requests.
- Links sin expiración.
- Falta de auditoría.
- Datos sensibles sin enmascarar.

## Backend obligatorio

- Validar permiso específico de export.
- Validar ownership/tenant.
- Aplicar filtros permitidos.
- Limitar rango de fechas.
- Limitar número máximo de filas.
- Procesar por job si es pesado.
- Guardar archivo en storage privado.
- Generar temporary URL.
- Auditar descarga y generación.

## CSV Injection

Sanitizar celdas que inicien con:

```text
= + - @ \t \r
```

Estrategia:

- Prefijar comilla simple `'`.
- O escapar según librería validada.
- Nunca confiar en nombres, emails, comentarios o campos libres.

## Columnas

Definir allowlist de columnas exportables. No usar `SELECT *`.

```php
$columns = ['id', 'name', 'status', 'created_at'];
```

## Jobs

Exports grandes deben:

- ejecutarse en cola `low` o `exports`;
- usar chunks;
- registrar progreso;
- manejar fallos;
- notificar al usuario;
- expirar archivo.

## Auditoría

Auditar:

- actor_id;
- company_id;
- tipo de export;
- filtros;
- cantidad de filas;
- archivo generado;
- IP/user agent;
- request_id;
- fecha.

## Checklist

- [ ] Permiso de export validado.
- [ ] companyId desde contexto seguro.
- [ ] Filtros limitados.
- [ ] Columnas allowlist.
- [ ] Datos sensibles enmascarados si aplica.
- [ ] CSV Injection mitigado.
- [ ] Job usado si es pesado.
- [ ] Storage privado.
- [ ] URL temporal.
- [ ] Auditoría completa.
