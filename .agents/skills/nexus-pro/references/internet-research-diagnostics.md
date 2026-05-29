# Internet Research & External Diagnostics

**NEXUS-PRO** debe ser capaz de investigar en internet cuando encuentre errores complejos, problemas de conexión, fallos de acoplamiento, librerías deprecadas o bugs desconocidos proporcionados por cualquier programador. Esto garantiza que la solución propuesta sea profesional, validada por la comunidad y libre de "alucinaciones" (alucinaciones de la IA).

## 1. Cuándo Investigar en Internet
- **Stack traces desconocidos:** Si el error o excepción no es obvio o involucra un paquete de terceros.
- **Errores de conexión:** Timeout, fallos de red, CORS, SSL, o rechazos de conexión que puedan estar relacionados con configuraciones de entorno específicas o infraestructura cloud.
- **Errores de acoplamiento (Coupling):** Problemas de compatibilidad entre versiones de librerías (ej. React 19 vs dependencias antiguas, o dependencias de Laravel en conflicto).
- **Casos límite (Edge Cases):** Comportamientos anómalos o bugs reportados en issues de GitHub, foros (StackOverflow) o documentación oficial.

## 2. Herramientas a Utilizar
El agente debe aprovechar proactivamente herramientas de búsqueda y MCP (por ejemplo, `perplexity-ask` o `search_web`) para buscar información externa en tiempo real.
- **Search Web / Perplexity:** Formular consultas precisas incluyendo el stack trace específico, los frameworks involucrados (con sus versiones) y el mensaje de error exacto.
- **Validación Oficial:** Priorizar soluciones provenientes de la documentación oficial, repositorios verificados de GitHub o foros reconocidos.

## 3. Flujo de Diagnóstico con Investigación Externa
1. **Detección:** El programador/usuario reporta un error enviando capturas de pantalla, descripciones o logs.
2. **Extracción y Sintetización:** Extraer el mensaje de error crítico, descartando el ruido del framework. Identificar las versiones del ecosistema afectado.
3. **Investigación Autónoma (Search):** Ejecutar llamadas a las herramientas de búsqueda web (ej. `[Mensaje de error exacto] Laravel 11 PostgreSQL connection error`).
4. **Análisis de Resultados:** Interpretar los resultados de internet para deducir la causa raíz estructural (ej. "Es un bug de la versión X", o "Falta configuración de timeout en el proxy", etc.).
5. **Formulación de Solución Profesional:** Entregar al usuario una respuesta estructurada que contenga:
   - **Causa Raíz Explicada:** Breve explicación basada en la investigación.
   - **Solución Propuesta:** Código limpio, arquitectónicamente correcto y profesional para resolverlo de forma permanente.
   - **Validación de Fuentes:** Mencionar si la solución proviene de un estándar oficial o de un workaround conocido en la comunidad.

## 4. Reglas Anti-Alucinación en Diagnósticos
- **Prohibido adivinar:** NUNCA inventar configuraciones o métodos mágicos para un error de terceros sin haber verificado en internet si la causa no es trivial.
- **Workarounds Documentados:** Si la investigación revela que es un bug abierto (unresolved) en la librería, proporcionar el "workaround" más aceptado y añadir un comentario explícito en el código propuesto: `// TODO: Workaround for [Librería] issue #[Número]. Remove after it's fixed.`
