# Prompt de revisión de seguridad para Antigravity

Actúa usando el skill `nexus-pro`.

Necesito una revisión de seguridad profunda del siguiente módulo, endpoint o flujo:

```text
[PEGAR RUTA, ARCHIVOS O DESCRIPCIÓN DEL FLUJO]
```

## Alcance

Revisa como auditor técnico enterprise:

- Autenticación.
- Autorización.
- Ownership.
- Validación.
- IDOR.
- SQL Injection.
- XSS.
- CSRF.
- Brute Force.
- DDoS.
- Mass Assignment.
- SSRF.
- Insecure Deserialization.
- API Key exposure.
- Headers seguros.
- Logs seguros.
- Secrets management.
- Roles y permisos.
- Exposición de datos.
- Rate limiting.
- Manejo de errores.

## Laravel

Verifica:

- Controllers sin queries.
- FormRequest.
- Policies/Gates.
- `$fillable`.
- `companyId` desde token/contexto autenticado.
- Queries filtradas por empresa.
- Transacciones.
- Logs seguros.
- Excepciones controladas.
- Uploads validados si aplica.

## React

Verifica:

- No hay secretos en frontend.
- No se renderiza HTML inseguro.
- No hay `dangerouslySetInnerHTML` sin sanitización.
- Errores no revelan datos internos.
- Permisos frontend no sustituyen backend.
- Inputs y uploads están limitados.

## Entrega esperada

Devuélveme un reporte con:

1. Riesgos críticos.
2. Riesgos altos.
3. Riesgos medios.
4. Riesgos bajos.
5. Evidencia por archivo o flujo.
6. Recomendación técnica.
7. Cambios concretos sugeridos.
8. Checklist final de seguridad.
