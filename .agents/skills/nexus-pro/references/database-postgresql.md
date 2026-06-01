# PostgreSQL Database Reference

## Propósito

Este documento define las directrices y estándares avanzados de diseño, modelado, optimización e infraestructura para PostgreSQL dentro de NEXUS-PRO. Está diseñado para garantizar la integridad referencial, el aislamiento multiempresa de alto desempeño y la escalabilidad de datos.

---

## 🏗️ Modelado Avanzado e Índices

### Claves Primarias y UUIDs
PostgreSQL maneja de forma óptima los tipos UUID nativos (`uuid`).
- **Uso Estándar**: Para tablas transaccionales de alto volumen que requieran identificadores universales, se debe utilizar el tipo `UUID` (y no `VARCHAR` o `TEXT` para representarlos).
- Para generación automática en DB, usar la función `gen_random_uuid()` disponible nativamente desde PostgreSQL 13+.
- Si se requiere orden cronológico natural por clave, preferir el estándar UUID v7 si la capa de aplicación lo soporta.

### Índices Especializados
PostgreSQL ofrece diversos tipos de índices además de B-Tree. Deben aplicarse según el caso:
- **GIN (Generalized Inverted Index)**: Obligatorio para buscar llaves, paths o valores dentro de columnas tipo `JSONB`.
- **GiST / BRIN**: BRIN es extremadamente útil para tablas ordenadas cronológicamente gigantescas (ej. tablas de logs de auditoría por timestamp) ya que consumen una fracción mínima del tamaño de un B-Tree.
- **Índices Parciales (Filtros)**: Crear índices solo para valores relevantes (ej. transacciones activas o no procesadas):
  ```sql
  CREATE INDEX idx_pending_transactions ON transactions (id) WHERE status = 'pending';
  ```
- **Índices Compuestos**: El orden de las columnas en un índice compuesto B-Tree es crítico. Colocar primero los campos que filtran por igualdad estricta (ej. `company_id`) y al final los campos de rango o ordenamiento (ej. `created_at`).

---

## ⚡ Manejo de JSONB y Estructuras No Relacionales

PostgreSQL maneja JSON semiestructurado mediante el tipo `JSONB` (binario pre-analizado).

### Reglas de Uso de JSONB
1. **No abusar**: No transformar PostgreSQL en un reemplazo de MongoDB. Los datos estructurados con relaciones sólidas deben seguir siendo columnas relacionales estándar.
2. **Índices GIN**: Para consultas rápidas dentro de JSONB, usar el operador de contención (`@>`) y crear un índice GIN:
  ```sql
  CREATE INDEX idx_users_settings_gin ON users USING gin (settings);
  -- Consulta optimizada:
  SELECT * FROM users WHERE settings @> '{"theme": "dark"}';
  ```
3. **JSON vs JSONB**: Usar siempre `JSONB` en lugar de `JSON` para optimizar consultas de lectura y permitir indexación. `JSON` solo se tolera si el uso es puramente de inserción y retorno rápido sin búsquedas internas.

---

## 📂 Declarative Partitioning (Particionamiento de Tablas)

Cuando una tabla supera las 10 millones de filas o se degrada la performance de los índices B-Tree, se debe aplicar particionamiento declarativo:

- **Estrategias**:
  - **RANGE**: Particionar tablas por fechas (ej. logs, transacciones financieras por mes o año).
  - **LIST**: Particionar por identificadores lógicos (ej. por grupo de tenants o regiones).
- **Regla de integridad**: Las llaves primarias en tablas particionadas deben incluir la columna de partición (ej. si se particiona por rango de fechas `created_at`, la PK debe ser `(id, created_at)`).

---

## 🛡️ Aislamiento de Tenants (Multi-Tenant)

### 1. Schema-per-Tenant (Esquemas Separados)
- Cada tenant tiene su propio esquema de base de datos dentro de la misma base física.
- Se aísla el acceso cambiando el `search_path` de la conexión del usuario:
  ```sql
  SET search_path TO tenant_company_a, public;
  ```
- Simplifica la seguridad y permite respaldar esquemas individuales (`pg_dump --schema=...`).

### 2. Row-Level Security (RLS)
- El aislamiento de datos se maneja a nivel de motor de base de datos a través de políticas RLS automáticas.
- **Obligatorio**: Definir una política en cada tabla para que filtre de acuerdo a la variable de sesión seteada:
  ```sql
  ALTER TABLE orders ENABLE ROW LEVEL SECURITY;
  CREATE POLICY order_tenant_policy ON orders 
  USING (company_id = current_setting('app.current_company_id', true));
  ```

---

## 🔄 Migraciones Concurrentes y Cuidado con Bloqueos

Las migraciones de PostgreSQL bloquean tablas completas para lecturas/escrituras en operaciones comunes. Para evitar downtime en producción:

### Creación de Índices sin Bloqueos
- **Prohibido**: Ejecutar `CREATE INDEX` simple sobre tablas con alta concurrencia de escrituras.
- **Obligatorio**: Usar `CREATE INDEX CONCURRENTLY`. Esto crea el índice en segundo plano sin bloquear inserciones, actualizaciones o eliminaciones de filas.
  *(Nota: En Laravel, usar `->algorithm('concurrently')` en las migraciones).*

### Autovacuum y Mantenimiento
- PostgreSQL marca filas eliminadas/actualizadas como "muertas" (dead tuples). El `autovacuum` se encarga de limpiarlas para recuperar espacio y reestructurar índices.
- Si una tabla tiene un flujo de actualización constante y masivo, ajustar los parámetros de autovacuum en esa tabla específica para ser más agresivos (`autovacuum_vacuum_scale_factor = 0.05`).

---

## 🩺 Checklist de Optimización PostgreSQL

Antes de entregar a producción, verificar:

- [ ] Todas las queries críticas tienen su respectivo índice compuesto.
- [ ] La creación de índices nuevos en producción utiliza `CONCURRENTLY`.
- [ ] Las consultas complejas que involucran agrupaciones o agregaciones hacen uso de índices index-only scan (cobertura total).
- [ ] El uso de `JSONB` está justificado e indexado con GIN si se filtra por sub-propiedades.
- [ ] No existen queries que realicen un `SELECT *` innecesario (especialmente sobre columnas `TEXT` largas o `JSONB` pesadas).
- [ ] RLS o filtros manuales por `company_id` están implementados y validados en todas las tablas sensibles del tenant.
