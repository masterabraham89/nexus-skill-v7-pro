# Análisis de Radio de Impacto (Blast Radius Analysis)

Al modificar archivos base, funciones compartidas o repositorios "core", existe un alto riesgo de romper otras partes del sistema inadvertidamente. **NEXUS-PRO** debe ejecutar un Análisis de Radio de Impacto antes de aplicar cambios estructurales.

## Protocolo Obligatorio:
1. **Identificación Core:** Identificar si la clase, función, interfaz o tabla de base de datos a modificar es compartida entre múltiples dominios (ej. `SyncOrchestrator`, `BaseRepository`, `AuthService`).
2. **Escaneo del Proyecto:** Utilizar proactivamente la herramienta `grep_search` para buscar todas las instancias y referencias de esa función/clase en todo el código base.
3. **Mapeo de Impacto:** Listar todos los módulos afectados.
4. **Notificación al Usuario:** Antes de realizar el cambio destructivo o de refactor, informar al usuario: *"Atención: Modificar X afectará también a Y y Z. Procederé a actualizar todos los puntos de uso para mantener la consistencia."*
5. **Ejecución Segura:** Actualizar la función core y refactorizar en cascada todas sus implementaciones y llamadas en el mismo lote de trabajo.

## Reglas:
- NUNCA cambiar la firma de un método base sin antes confirmar con `grep_search` dónde se está usando.
- Si el impacto es demasiado grande (ej. cientos de archivos), proponer un enfoque "Expand & Contract" (crear el nuevo método, migrar progresivamente y luego eliminar el antiguo).
