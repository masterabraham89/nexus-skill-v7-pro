# OpenAPI Reference

## Propósito

Este documento define cómo documentar APIs usando OpenAPI 3.1 en NEXUS-PRO.

---

## Reglas

- Toda API pública o interna crítica debe documentarse.
- Los schemas deben reflejar Resources reales.
- Los errores deben estar documentados.
- La autenticación debe estar declarada.
- Los parámetros deben tener tipos y límites.
- Los ejemplos deben ser realistas y no contener datos sensibles.

---

## Estructura mínima

```yaml
openapi: 3.1.0
info:
  title: Mi API Enterprise
  version: 1.0.0
paths: {}
components:
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: Sanctum
```

---

## Endpoint schema

Debe incluir:

- Summary.
- Description.
- Security.
- Parameters.
- RequestBody.
- Responses.
- Error examples.

---

## Error schema

```yaml
ErrorResponse:
  type: object
  properties:
    success:
      type: boolean
      example: false
    message:
      type: string
    error:
      type: object
      properties:
        code:
          type: string
        details:
          type: object
```

---

## Contract testing

Usar contratos para validar:

- El backend responde lo documentado.
- El frontend consume tipos correctos.
- No se rompen respuestas sin aviso.

---

## Typed API contracts

Cuando sea posible:

- Generar tipos TypeScript desde OpenAPI.
- Evitar duplicar interfaces manuales.
- Versionar cambios.

---

## Checklist OpenAPI

- [ ] Ruta documentada.
- [ ] Auth documentada.
- [ ] Request body documentado.
- [ ] Response documentado.
- [ ] Errores documentados.
- [ ] Paginación documentada.
- [ ] Examples seguros.
- [ ] Types actualizados.
