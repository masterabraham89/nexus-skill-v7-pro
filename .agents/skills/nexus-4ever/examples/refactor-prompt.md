# Prompt de refactorización para Antigravity

Actúa usando el skill `nexus-4ever`.

Necesito refactorizar el siguiente módulo sin cambiar su comportamiento funcional. Antes de modificar, analiza la estructura actual y aplica arquitectura enterprise.

## Objetivo

Refactorizar el código para que cumpla:

- Una responsabilidad por archivo.
- Controllers sin acceso directo a DB.
- Services con reglas de negocio.
- Repositories con queries.
- FormRequest para validación.
- Policies/Gates para autorización.
- Resources para respuestas API.
- `companyId` siempre desde token/contexto autenticado.
- Validación de ownership.
- Transacciones con rollback cuando existan operaciones críticas.
- Logs seguros.
- Límites razonables de líneas por archivo.

## Frontend, si aplica

- Components sin fetch directo.
- Hooks sin JSX.
- Modales independientes.
- Services para llamadas HTTP.
- Types por módulo.
- `useSWR` o `react-query` para datos remotos.
- Cleanup obligatorio en `useEffect`.
- Estados de loading, error, empty y success.

## Instrucciones

1. Revisa los archivos actuales.
2. Identifica responsabilidades mezcladas.
3. Propón la nueva división de archivos.
4. Implementa el refactor.
5. No cambies nombres públicos de rutas o contratos salvo que sea estrictamente necesario.
6. Mantén compatibilidad con el comportamiento actual.
7. Agrega o sugiere pruebas.
8. Ejecuta una revisión de seguridad antes de entregar.

## Validaciones obligatorias

- No debe quedar lógica de negocio en controllers.
- No debe quedar query directa en controllers.
- No debe existir IDOR.
- No debe confiarse en `company_id` enviado por frontend.
- No debe haber Mass Assignment.
- No debe haber logs con datos sensibles.
- No debe aumentar complejidad innecesaria.

## Entrega esperada

Devuélveme:

- Archivos modificados.
- Resumen técnico del refactor.
- Responsabilidad de cada archivo.
- Riesgos detectados.
- Checklist de seguridad.
- Pruebas realizadas o recomendadas.
