# Database Reference

## Propósito

Este documento define las prácticas PostgreSQL obligatorias para NEXUS-PRO. La base de datos debe ser consistente, segura, auditable y optimizada para crecimiento.

---

## PostgreSQL best practices

### Reglas base

- Usar tipos correctos.
- Usar constraints.
- Usar índices según consultas reales.
- Usar transacciones.
- Usar prepared statements mediante ORM/query builder.
- Evitar queries N+1.
- Usar paginación.
- Auditar cambios sensibles.
- Aplicar mínimos privilegios.
- Habilitar SSL en conexiones productivas.
- Monitorear queries lentas.

---

## Prepared statements

Laravel Eloquent y Query Builder usan bindings. Debe preferirse este enfoque.

Correcto:

```php
User::query()
    ->where('email', $email)
    ->first();
```

Incorrecto:

```php
DB::select("SELECT * FROM users WHERE email = '$email'");
```

Si se usa SQL crudo, debe tener bindings:

```php
DB::select(
    'SELECT * FROM users WHERE email = ?',
    [$email]
);
```

---

## SSL

En producción:

- Usar SSL/TLS para conexiones a PostgreSQL.
- Validar certificados cuando la infraestructura lo permita.
- No exponer PostgreSQL públicamente.
- Restringir acceso por red privada, firewall o security groups.
- Usar usuarios separados por aplicación, migraciones y reporting si aplica.

---

## Índices

Crear índices en columnas usadas en:

- `WHERE`.
- `JOIN`.
- `ORDER BY`.
- Foreign keys.
- Búsquedas frecuentes.
- Filtros por `company_id`.
- Fechas en reportes.
- Estados de procesos.

Ejemplo:

```sql
CREATE INDEX idx_orders_company_status_created
ON orders (company_id, status, created_at DESC);
```

### Reglas

- No crear índices innecesarios.
- Revisar cardinalidad.
- Evitar índices duplicados.
- Considerar índices compuestos según filtros reales.
- Considerar índices parciales para estados frecuentes.

---

## EXPLAIN ANALYZE

Toda query crítica debe analizarse con:

```sql
EXPLAIN ANALYZE
SELECT ...
```

Revisar:

- Seq Scan inesperado.
- Uso de índices.
- Tiempo total.
- Rows estimadas vs reales.
- Sorts costosos.
- Nested loops problemáticos.
- Hash joins grandes.
- Buffers si está disponible.

---

## pg_stat_statements

En entornos productivos o staging avanzado, habilitar `pg_stat_statements` para detectar:

- Queries más lentas.
- Queries más frecuentes.
- Queries con mayor tiempo acumulado.
- Queries con alto consumo de recursos.
- Queries con variabilidad anómala.

---

## JSONB

Usar JSONB solo cuando aporte valor real:

Permitido:

- Metadata flexible.
- Configuraciones variables.
- Payloads de integraciones.
- Auditoría de cambios.
- Campos de baja frecuencia de búsqueda.

Evitar JSONB para:

- Datos relacionales centrales.
- Campos que requieren joins frecuentes.
- Datos que necesitan constraints estrictos.
- Información crítica sin validación.

Si se consulta JSONB frecuentemente, considerar índices GIN.

---

## Particionado

Considerar particionado cuando existan tablas muy grandes:

- Logs.
- Auditoría.
- Eventos.
- Historiales.
- Transacciones por fecha.
- Datos multiempresa de alto volumen.

Estrategias:

- Por fecha.
- Por empresa si el volumen lo justifica.
- Por estado en casos específicos.

No particionar prematuramente.

---

## Backups

### Reglas

- Backups automáticos.
- Backups horarios para sistemas críticos.
- Retención definida.
- Pruebas de restauración.
- Cifrado en reposo.
- Cifrado en tránsito.
- Almacenamiento fuera del servidor principal.
- Documentar RPO y RTO.

---

## Retención

Definir políticas por tipo de dato:

- Logs técnicos.
- Auditoría.
- Datos personales.
- Archivos subidos.
- Tokens.
- Sesiones.
- Reportes temporales.
- Backups.

No conservar datos indefinidamente sin justificación.

---

## Usuarios con mínimos privilegios

Separar usuarios:

- App runtime.
- Migraciones.
- Reporting.
- Backups.
- Read-only analytics.

El usuario de app no debe tener privilegios innecesarios como superuser.

---

## Connection pooling

Usar pooling cuando haya concurrencia alta:

- PgBouncer.
- Pool administrado del proveedor cloud.
- Configuración correcta de max connections.
- Timeouts.
- Reutilización de conexiones.

Evitar abrir conexiones excesivas desde workers.

---

## Auditoría

Auditar acciones sensibles:

- Cambios de roles.
- Cambios de permisos.
- Cambios de datos fiscales.
- Eliminaciones.
- Exportaciones.
- Login/logout.
- Fallos de autorización.
- Cambios de configuración.
- Operaciones de sync.

La auditoría debe incluir:

- Usuario.
- Empresa.
- Acción.
- Entidad.
- Antes/después cuando aplique.
- IP/user agent si corresponde.
- Timestamp UTC.
- Request ID.

---

## Transacciones

Usar transacciones para:

- Operaciones multi-tabla.
- Correlativos.
- Inventario.
- Facturación.
- Pagos.
- Auditoría acoplada.
- Sincronizaciones críticas.

---

## Checklist database

Antes de entregar:

- [ ] Queries filtran por `company_id` cuando aplica.
- [ ] Índices revisados.
- [ ] No hay SQL concatenado.
- [ ] Paginación aplicada.
- [ ] N+1 evitado.
- [ ] Transacciones aplicadas.
- [ ] Constraints considerados.
- [ ] Auditoría considerada.
- [ ] Backups no afectados.
- [ ] Mínimos privilegios respetados.
