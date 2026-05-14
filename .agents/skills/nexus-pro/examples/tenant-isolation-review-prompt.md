# Prompt de revisión multiempresa para Antigravity

Actúa usando el skill `nexus-pro`.

Necesito revisar aislamiento multiempresa en el siguiente flujo:

```text
[PEGAR ENDPOINT, MÓDULO, JOB, EXPORT O REPORTE]
```

Verifica obligatoriamente:

- `companyId` desde token/contexto autenticado.
- Ningún `company_id` confiado desde frontend.
- Repositories filtrando por empresa.
- Policies validando ownership.
- Jobs respetando empresa.
- Cache keys con companyId.
- Exports filtrados por empresa.
- Reportes filtrando antes de agregar.
- Tests de usuario empresa A contra datos empresa B.

Entrega:

1. Riesgos encontrados.
2. Archivos afectados.
3. Cambios necesarios.
4. Tests recomendados.
5. Checklist final de tenant isolation.
