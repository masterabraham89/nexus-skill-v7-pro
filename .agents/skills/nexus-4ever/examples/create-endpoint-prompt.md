# Prompt: crear endpoint enterprise

Actúa usando el skill `nexus-4ever` versión enterprise.

Necesito crear un endpoint para:

```text
[DESCRIBIR CASO DE USO]
```

## Reglas obligatorias

- Revisa rutas, controllers, services, repositories y patterns existentes antes de crear archivos.
- No inventes columnas, relaciones ni permisos.
- Usa Controller -> Service -> Repository.
- Usa FormRequest.
- Usa Policy/Gate.
- Usa Resource.
- `companyId` debe venir desde token/contexto autenticado.
- Valida ownership.
- Define contrato request/response.
- Documenta OpenAPI si el proyecto lo usa.
- Agrega o sugiere feature test.
- Revisa seguridad y performance.

## Entrega esperada

1. Diseño del endpoint.
2. Archivos creados/modificados.
3. Código.
4. Contrato API.
5. Checklist security.
6. Pruebas recomendadas.
