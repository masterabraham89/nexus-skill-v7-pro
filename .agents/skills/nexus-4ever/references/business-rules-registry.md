# Business Rules Registry

## Propósito

Este documento centraliza reglas de negocio y reglas técnicas obligatorias. El objetivo es evitar que reglas críticas queden dispersas o se contradigan.

## Reglas globales obligatorias

### Multiempresa

- `companyId` nunca viene del cliente como fuente de verdad.
- `companyId` siempre viene del token autenticado o contexto server-side.
- Todo recurso privado debe validar ownership.
- Toda query privada debe filtrar por empresa.

### Arquitectura

- Controllers no tocan DB.
- Services contienen negocio.
- Repositories contienen queries.
- Components no hacen fetch directo.
- Hooks no contienen JSX.
- Modales independientes.

### Sync

- Sync debe ser idempotente.
- Upserts siempre en chunks de 200.
- Deduplicación por empresa + identificador externo.
- Locks para sync concurrente.

### Seguridad

- Revisar autorización antes de entregar.
- No logs con secretos.
- No datos de producción en desarrollo.
- No exponer API keys en frontend.

### Datos financieros/documentos

- Facturas no se eliminan físicamente sin política explícita.
- Documentos legales se anulan, no se borran.
- Exports se auditan.

### Performance

- Listados paginados.
- No N+1.
- Jobs para procesos pesados.
- Índices para filtros críticos.

## Formato para nuevas reglas

```markdown
## [Código de regla]

- Regla:
- Motivo:
- Aplica a:
- Ejemplo correcto:
- Ejemplo prohibido:
- Tests requeridos:
```

## Checklist

- [ ] Nueva regla documentada.
- [ ] Regla no contradice otra existente.
- [ ] Tests considerados.
- [ ] Impacto comunicado.
- [ ] Documentación del módulo actualizada.
