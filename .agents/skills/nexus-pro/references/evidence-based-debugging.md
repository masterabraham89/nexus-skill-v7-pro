# Evidence-Based Debugging (Protocolo de Triangulación)

Esta directiva corrige el comportamiento de IA de "Adivinar y Parchear" o de entrar en "Bucles Infinitos de Búsqueda". Antigravity tiene PROHIBIDO adivinar el origen de un error o asumir esquemas de base de datos sin pruebas empíricas.

## 1. Triangulación Obligatoria
Ante un reporte de error, Antigravity debe seguir el rastro sistemático:
1. Identificar el punto de entrada (Ej: Ruta API en `routes/api.php` o acción UI en React).
2. Trazar la ejecución capa por capa (Controller -> Service -> Repository / Component -> Hook -> Fetcher).
3. **Verificar la Realidad:** Usar herramientas (`view_file`, `grep_search`) para comprobar que las columnas, variables o endpoints REALMENTE EXISTEN en los archivos destino antes de proponer una solución.

## 2. Cero Asunciones (No Blind Patching)
Si el backend arroja un error sobre una columna faltante (ej. `unknown column 'tax_id'`), no asumas que el frontend la está enviando mal o que la base de datos la requiere por error. 
- Verifica el payload enviado por el frontend.
- Verifica el FormRequest del backend.
- Verifica la migración de la base de datos.
**Solo cuando tengas evidencia visual (leída con herramientas) de la desconexión en las tres partes, procedes a escribir el parche.**

## 3. Límite de Exploración (Ahorro de Tokens)
Para evitar que Antigravity escanee todo el proyecto dando vueltas en círculos consumiendo recursos y tiempo:
- Antigravity tiene un límite de **máximo 3 pasos de lectura profunda** por intento de diagnóstico.
- Si tras rastrear el Controller, el FormRequest y la Migración (o su equivalente en Frontend) no se localiza el error evidente, la IA **DEBE DETENERSE**.
- En lugar de seguir leyendo archivos aleatorios, debe explicarle al usuario lo que encontró y pedirle que comparta un log específico o el archivo exacto donde ocurre la falla.

## 4. La Solución NO siempre es Código
A veces el error es que falta ejecutar `php artisan migrate`, `npm run build`, o purgar la caché de Redis. Antes de escribir código para evadir un error, analiza si es un problema de estado del entorno.
