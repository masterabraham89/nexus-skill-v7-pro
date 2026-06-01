# MySQL Database Reference

## Propósito

Este documento define las prácticas de diseño, migración, estructuración y optimización de MySQL / MariaDB obligatorias para NEXUS-PRO. Se enfoca en garantizar el rendimiento de lecturas/escrituras, la consistencia transaccional en InnoDB y la seguridad de datos en producción.

---

## ⚙️ Storage Engine y Configuración Base

### InnoDB Obligatorio
Toda tabla en MySQL debe usar el motor **InnoDB**. Se prohíbe el uso de MyISAM o Memory para datos persistentes debido a la falta de soporte transaccional y bloqueo a nivel de tabla.

- En las migraciones de Laravel/NestJS, asegurar que el engine por defecto esté configurado a `InnoDB`.
- Usar el charset `utf8mb4` y la colación `utf8mb4_unicode_ci` de manera estándar para soportar de forma nativa caracteres complejos y emojis sin pérdida de datos.

### Aislamiento de Transacciones
- Por defecto, el nivel de aislamiento en InnoDB es `REPEATABLE READ`. 
- Si se detecta un alto volumen de transacciones concurrentes con riesgos de **Deadlocks**, considerar configurar el aislamiento a `READ COMMITTED` (ideal para aplicaciones SaaS que no requieren bloqueos de rango estrictos).

---

## 🔑 Estrategia de Primary Keys e Índices

### Claves Primarias: BIGINT vs UUID
En MySQL InnoDB, la tabla se almacena físicamente ordenada por su Clave Primaria (Índice Clúster). Por ende:

- **Recomendado (Estándar)**: Usar un entero autoincremental `BIGINT UNSIGNED` como primary key física. Esto optimiza enormemente las inserciones y previene la fragmentación de páginas de disco.
- **UUID en MySQL**: Si es obligatorio usar UUID por negocio:
  - Evitar almacenar UUIDs como texto de 36 caracteres (`CHAR(36)`), ya que degrada la performance de los índices B-Tree debido a su distribución aleatoria.
  - Almacenar los UUIDs en formato binario (`BINARY(16)`). En Laravel, usar `uuid` binario o en NestJS usar TypeORM con transformadores binarios.
  - Usar UUIDs secuenciales (UUID v7 o versiones combinadas) si es posible para mantener el orden de inserción.

### Índices B-Tree
- MySQL mapea los índices secundarios a la Clave Primaria Clúster. Por lo tanto, un índice secundario grande incrementará el consumo de memoria total.
- **Index-Covering**: Diseñar índices compuestos que incluyan los campos seleccionados en el `SELECT` para evitar el acceso al índice clúster principal (evitando un "Bookmark Lookup").
- Proteger contra la **conversión implícita**: Asegurarse de que los tipos en la cláusula `WHERE` coincidan exactamente con la columna indexada (ej. comparar `VARCHAR` indexado con strings, no con enteros), de lo contrario MySQL ignorará el índice.

---

## 🏗️ Topologías de Base de Datos y Multi-Tenancy

### 1. Base de datos única con Shared Schema
- Todos los clientes comparten la misma DB. Las tablas se filtran por `company_id` (o la columna del tenant definida).
- **Obligatorio**: Todos los índices de filtrado deben ser compuestos e iniciar con la columna del tenant:
  ```sql
  CREATE INDEX idx_orders_company_status ON orders (company_id, status);
  ```

### 2. Database-per-Tenant (Base de datos por cliente)
- Ideal para cumplimiento de privacidad estricto.
- El backend debe manejar un gestor de conexiones dinámico (Connection Switcher).
- Las migraciones deben poder ejecutarse en bucle sobre cada base de datos tenant de manera segura.

### 3. Read Replicas (Réplicas de Lectura)
- Separar la conexión de Escritura (Master) de la de Lectura (Replica).
- Asegurar que la lógica de negocio tolere el lag de replicación en consultas de lectura inmediata tras una escritura.

---

## ⚠️ Migraciones Seguras (Online DDL)

Las migraciones sobre tablas grandes en producción pueden bloquear la base de datos entera.

### Reglas para Tablas Grandes (>1M filas)
- **Bloqueos de metadatos**: Evitar DDLs directos si hay transacciones activas largas, ya que pueden acumularse peticiones y tumbar el pool de conexiones.
- Usar las opciones nativas de Online DDL de InnoDB cuando sea posible:
  ```sql
  ALTER TABLE users ADD COLUMN phone VARCHAR(20), ALGORITHM=INPLACE, LOCK=NONE;
  ```
- Si la migración es extremadamente masiva (ej. reconstrucción de tablas o cambios de tipo de columna), usar herramientas externas de copia de tabla en segundo plano como:
  - **gh-ost** (GitHub Online Schema Migrator)
  - **pt-online-schema-change** (Percona Toolkit)

### Rollback Strategy
Toda migración hacia adelante (`up`) debe tener una función inversa de rollback (`down`) probada en ambientes locales que elimine las columnas/tablas creadas sin corromper la integridad referencial.

---

## 🩺 Checklist de Optimización MySQL

Antes de liberar cambios de base de datos a producción, verificar:

- [ ] Todas las tablas usan `InnoDB` con `utf8mb4_unicode_ci`.
- [ ] No existen índices duplicados (ej. indexar `A` y también crear índice compuesto `(A, B)`; el índice unitario en `A` es redundante).
- [ ] La columna `company_id` está indexada adecuadamente en todas las tablas transaccionales.
- [ ] Las consultas con `LIKE '%termino'` se evitan, ya que no usan índices normales B-Tree. Usar búsquedas específicas o Full-Text Indexing en su lugar.
- [ ] Las queries críticas han sido ejecutadas con `EXPLAIN` confirmando un `type` distinto de `ALL` (Full Table Scan).
- [ ] Los bloqueos por transacciones (ej. `SELECT ... FOR UPDATE`) se liberan inmediatamente y cubren el menor número de registros posible.
