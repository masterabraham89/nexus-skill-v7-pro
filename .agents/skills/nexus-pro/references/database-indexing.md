# Database Performance & Indexing (Escalabilidad de DB)

En NEXUS-PRO, las bases de datos deben estar preparadas para soportar 10+ millones de filas sin degradación de rendimiento.

## 1. Índices Obligatorios en Migraciones
Toda migración que agregue una nueva tabla DEBE cumplir las siguientes reglas de indexación:
- **Foreign Keys:** Toda columna que termine en `_id` debe tener un índice (ej. `$table->index('company_id');`).
- **Estados/Tipos:** Columnas usadas comúnmente en filtros `WHERE` (ej. `status`, `type`, `is_active`) DEBEN tener un índice.
- **Búsqueda Frecuente:** Columnas como `email`, `slug`, o `document_number` deben ser índices o únicas.

## 2. Prevención de Full Table Scans
Prohibido generar consultas de bases de datos masivas que hagan "Full Table Scan". Siempre se debe prever el índice a nivel de migración.

## 3. Índices Compuestos para Multi-tenant
Si la tabla usa `company_id` o `tenant_id`, las restricciones `unique` deben ser compuestas. Ej: `$table->unique(['company_id', 'email']);` para que correos se puedan repetir en diferentes empresas.
