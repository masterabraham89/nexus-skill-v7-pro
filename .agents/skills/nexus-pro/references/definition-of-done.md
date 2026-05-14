# Definition of Done Reference

## Propósito

Este documento define cuándo una tarea puede considerarse terminada. En NEXUS-PRO, “funciona en mi máquina” no es suficiente.

## Definition of Done general

Una tarea está terminada solo si:

- compila;
- pasa lint;
- pasa typecheck si aplica;
- respeta arquitectura;
- tiene validación;
- tiene autorización;
- valida ownership;
- maneja errores;
- no expone datos sensibles;
- no rompe contratos;
- tiene pruebas o estrategia de pruebas;
- considera performance;
- considera observabilidad;
- considera rollback si afecta producción;
- actualiza documentación si corresponde.

## Backend DoD

- Controller sin DB.
- Service con negocio.
- Repository con queries.
- FormRequest.
- Policy/Gate.
- Resource si aplica.
- Transactions si aplica.
- Logs seguros.
- Tests de feature/unit recomendados o implementados.

## Frontend DoD

- Componentes sin fetch directo.
- Hooks sin JSX.
- Servicios para API.
- Types definidos.
- Estados UI completos.
- Accesibilidad básica.
- Cleanup en useEffect.
- Sin secretos.
- Sin `any` injustificado.

## Security DoD

- Auth.
- Authorization.
- Ownership.
- IDOR prevenido.
- Mass Assignment prevenido.
- SQL Injection prevenido.
- XSS revisado.
- Logs seguros.
- Secrets seguros.

## Production DoD

- Migración segura.
- Rollback disponible.
- Health checks.
- Monitoreo.
- Alertas si crítico.
- Feature flag si aplica.
- Runbook si aplica.

## Checklist final

- [ ] Funcionalidad correcta.
- [ ] Arquitectura respetada.
- [ ] Seguridad validada.
- [ ] Performance revisada.
- [ ] Tests considerados.
- [ ] Observabilidad considerada.
- [ ] Documentación actualizada.
- [ ] Rollback considerado.
- [ ] Riesgos comunicados.
