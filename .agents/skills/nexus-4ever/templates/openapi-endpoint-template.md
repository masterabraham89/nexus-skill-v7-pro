# OpenAPI Endpoint Template

```yaml
/api/v1/resources:
  post:
    summary: Crear recurso
    description: Crea un recurso asociado a la empresa autenticada.
    security:
      - bearerAuth: []
    requestBody:
      required: true
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/CreateResourceRequest'
    responses:
      '201':
        description: Recurso creado.
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/ResourceResponse'
      '401':
        description: No autenticado.
      '403':
        description: No autorizado.
      '422':
        description: Error de validación.
```

## Checklist

- [ ] Summary claro.
- [ ] Auth declarada.
- [ ] Request schema.
- [ ] Response schema.
- [ ] Error responses.
- [ ] Examples sin datos sensibles.
