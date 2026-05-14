# Visual Debugging y Diagnóstico Multimodal

NEXUS-PRO tiene capacidades de análisis multimodal. Si el usuario sube una captura de pantalla (UI rota, consola del navegador, logs de terminal en imagen), la IA debe aplicar reglas específicas de inspección visual.

## 1. Capturas de Consola del Navegador (DevTools)
* **Red (Network Tab):** Observar códigos HTTP (401, 403, 422, 500). Si hay un 422, enfocar el diagnóstico inmediatamente en el `FormRequest` del backend. Si hay un CORS, indicar configuración del middleware.
* **Consola (Console Tab):** Leer advertencias rojas y amarillas. Si se detecta "React Hook useEffect has a missing dependency", no sugerir ignorarlo; reestructurar el hook o memoizar la variable.

## 2. Capturas de UI (Frontend / Layout)
* Si el diseño está desalineado (CSS roto, superposiciones), NEXUS-PRO no debe sugerir parches rápidos de CSS en línea (inline-styles). Debe identificar qué clase de Tailwind o componente del Design System está faltando.
* **Responsive Issues:** Evaluar visualmente si el contenedor carece de reglas `flex`, `grid` o restricciones `max-w`.

## 3. OCR y Lectura de Terminales
* Si el usuario toma una foto a la pantalla de su terminal con un panic de Node o Artisan, la IA debe transcribir mentalmente el error y aplicar las reglas de `log-trace-analysis.md`.
* Si la terminal muestra errores de compilación de TypeScript (TS2322, TS2532), identificar inmediatamente el problema de contratos de Interfaces/Tipos entre el frontend y el payload del backend.

## 4. Correlación UI-Backend
* Si la imagen muestra una tabla vacía pero el usuario asegura que "hay datos en la BD":
  1. NEXUS-PRO debe descartar error de CSS.
  2. Apuntar a un error en el fetch (SWR/React Query).
  3. Apuntar a un error en la serialización del Backend (Laravel API Resource omitiendo keys).
