# Contract Testing Reference

## Propósito

Este documento define pruebas de contrato para evitar que backend y frontend se rompan entre sí. OpenAPI documenta el contrato; contract testing verifica que el contrato se cumple.

## Riesgos

- Backend cambia response y rompe frontend.
- Frontend envía payload diferente al esperado.
- Error schema inconsistente.
- Campo requerido desaparece.
- Tipos incompatibles.
- Versiones API sin compatibilidad.

## Reglas

- Todo endpoint público o consumido por frontend debe tener contrato.
- Responses deben validarse contra schema.
- Errores deben usar taxonomía estándar.
- Breaking changes requieren versión nueva o migración controlada.
- Deprecated fields deben mantenerse por periodo definido.

## OpenAPI como fuente

El contrato debe incluir:

- path;
- método;
- auth;
- permisos;
- request schema;
- response schema;
- error schema;
- ejemplos;
- códigos HTTP.

## Tests backend

Validar que responses reales cumplen schema.

Casos:

- 200/201.
- 400/422.
- 401.
- 403.
- 404.
- 409.
- 429 si aplica.

## Tests frontend

- Generar tipos desde OpenAPI cuando sea posible.
- Evitar tipos manuales divergentes.
- Validar payload antes de enviar en formularios críticos.
- Manejar todos los códigos de error documentados.

## Backward compatibility

Cambios compatibles:

- Agregar campo opcional.
- Agregar endpoint nuevo.
- Agregar enum solo si frontend tolera unknown.

Cambios incompatibles:

- Renombrar campo.
- Eliminar campo.
- Cambiar tipo.
- Cambiar estructura de error.
- Cambiar semántica de status code.

## Checklist

- [ ] Endpoint documentado.
- [ ] Request schema definido.
- [ ] Response schema definido.
- [ ] Error schema definido.
- [ ] Tests validan contrato.
- [ ] Frontend usa tipos alineados.
- [ ] Breaking changes evitados o versionados.
