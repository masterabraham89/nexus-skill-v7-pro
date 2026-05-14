# Enterprise Performance & Payload Strictness

NEXUS-PRO debe construir aplicaciones que soporten millones de registros sin ahogar la memoria del servidor ni el ancho de banda del cliente.

## 1. Anti-N+1 y Eager Loading Estricto
- **Regla:** Está prohibido realizar consultas SQL dentro de ciclos (loops).
- **Obligación:** Toda relación de base de datos (ej. un usuario y sus pedidos) debe cargarse de antemano usando *Eager Loading* (ej. `with('orders')` en Laravel o `include: { orders: true }` en Prisma).

## 2. Bloqueo de "SELECT *" (Payload Budget)
- **Regla:** Está prohibido retornar registros completos de bases de datos hacia las APIs públicas a menos que sea estrictamente necesario.
- **Obligación:** Las APIs deben retornar explícitamente solo los campos necesarios usando DTOs (Data Transfer Objects), *Resources* o seleccionando campos específicos (`.select('id', 'name', 'status')`). Nunca filtres los datos de forma insegura omitiéndolos solo en el frontend; omítelos desde la consulta SQL.

## 3. Paginación Obligatoria
- Las consultas que puedan retornar colecciones que crezcan en el tiempo (Logs, Usuarios, Productos, Pedidos) DEBEN usar paginación siempre. Nunca retornar colecciones completas (`.all()`).
