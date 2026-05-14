# Code Review Checklist

## Arquitectura

- [ ] El cambio respeta la arquitectura definida.
- [ ] Hay una responsabilidad por archivo.
- [ ] No hay archivos gigantes sin justificación.
- [ ] No hay lógica duplicada.
- [ ] Los nombres son claros y específicos.
- [ ] La solución no introduce abstracciones innecesarias.
- [ ] El código sigue patrones existentes del proyecto.

---

## Backend Laravel

- [ ] Controllers no tocan DB.
- [ ] Controllers delegan en services.
- [ ] Services contienen negocio.
- [ ] Repositories contienen queries.
- [ ] FormRequest valida entrada.
- [ ] Policies/Gates validan autorización.
- [ ] Resources transforman respuestas.
- [ ] Jobs se usan para procesos pesados.
- [ ] Transacciones aplicadas donde corresponde.
- [ ] Rollback garantizado.
- [ ] Upserts en chunks de 200.
- [ ] Sync con deduplicación.
- [ ] Correlativos seguros ante concurrencia.
- [ ] Logs estructurados y seguros.
- [ ] Errores controlados.

---

## Frontend React

- [ ] Components no hacen fetch directo.
- [ ] Hooks no contienen JSX.
- [ ] Modales independientes.
- [ ] Services encapsulan llamadas HTTP.
- [ ] Types definidos.
- [ ] `useSWR` o `react-query` usado para datos remotos.
- [ ] Cleanup obligatorio en `useEffect`.
- [ ] Loading state implementado.
- [ ] Error state implementado.
- [ ] Empty state implementado.
- [ ] Success feedback implementado.
- [ ] Sin `any` injustificado.
- [ ] Render performance revisado.
- [ ] Code splitting considerado.

---

## Seguridad

- [ ] Autenticación requerida.
- [ ] Autorización aplicada.
- [ ] Ownership validado.
- [ ] `companyId` desde token/contexto autenticado.
- [ ] No se confía en `company_id` del cliente.
- [ ] IDOR prevenido.
- [ ] SQL Injection prevenido.
- [ ] XSS revisado.
- [ ] CSRF considerado.
- [ ] Mass Assignment prevenido.
- [ ] SSRF revisado si hay URLs externas.
- [ ] Rate limiting aplicado si corresponde.
- [ ] Secrets no expuestos.
- [ ] Logs sin datos sensibles.
- [ ] Errores no revelan internals.
- [ ] Headers seguros considerados.
- [ ] Dependencias vulnerables revisadas.

---

## Base de datos

- [ ] Queries filtran por empresa.
- [ ] Índices considerados.
- [ ] Paginación aplicada.
- [ ] No hay N+1.
- [ ] No hay `SELECT *` innecesario.
- [ ] Constraints considerados.
- [ ] Transacciones usadas.
- [ ] Auditoría considerada.
- [ ] Backups/migraciones revisados.
- [ ] Mínimos privilegios respetados.

---

## Performance

- [ ] Query crítica revisada.
- [ ] `EXPLAIN ANALYZE` considerado.
- [ ] Cache considerada.
- [ ] Cache key incluye empresa si aplica.
- [ ] Jobs para cargas pesadas.
- [ ] Chunks de 200.
- [ ] Imágenes optimizadas.
- [ ] Bundle size revisado.
- [ ] Lazy loading aplicado si corresponde.
- [ ] Virtualización considerada en listas grandes.

---

## Testing

- [ ] Unit tests agregados o recomendados.
- [ ] Feature tests agregados o recomendados.
- [ ] Authorization test considerado.
- [ ] Ownership test considerado.
- [ ] Validation test considerado.
- [ ] Regression test considerado.
- [ ] E2E considerado para flujo crítico.
- [ ] Security tests considerados.
- [ ] Tests cubren caso feliz y fallos.

---

## Accesibilidad

- [ ] Labels en inputs.
- [ ] Contraste suficiente.
- [ ] Navegación por teclado.
- [ ] Focus visible.
- [ ] Modales accesibles.
- [ ] Errores accesibles.
- [ ] ARIA correcto.
- [ ] Alt en imágenes.
- [ ] Lighthouse/axe-core considerado.

---

## Privacidad

- [ ] Datos mínimos.
- [ ] Finalidad clara.
- [ ] PII protegida.
- [ ] Logs sin PII innecesaria.
- [ ] Retención considerada.
- [ ] No se usan datos de producción en desarrollo.
- [ ] Consentimiento considerado si aplica.
- [ ] Derechos ARCO considerados si aplica.

---

## CI/CD y despliegue

- [ ] Lint pasa.
- [ ] Typecheck pasa.
- [ ] PHPStan/Larastan considerado.
- [ ] Tests pasan.
- [ ] Build pasa.
- [ ] npm audit revisado.
- [ ] composer audit revisado.
- [ ] Variables de entorno revisadas.
- [ ] Migraciones seguras.
- [ ] Rollback preparado.
- [ ] Health checks definidos.

---

## Criterio final

El cambio puede aprobarse solo si:

- Funciona.
- Es seguro.
- Está separado por responsabilidades.
- Es mantenible.
- Es testeable.
- No rompe permisos.
- No expone datos.
- No degrada performance de forma evidente.
- Puede monitorearse en producción.

---

## PRO v7 adicional

### Plan y alcance

- [ ] Se aplicó Plan Before Code cuando correspondía.
- [ ] El cambio se mantuvo dentro del alcance.
- [ ] Deuda técnica fuera del alcance fue reportada, no modificada sin instrucción.

### Multiempresa

- [ ] Tests o revisión de tenant isolation.
- [ ] Jobs respetan companyId.
- [ ] Cache keys incluyen companyId si son privadas.
- [ ] Exports y reportes filtran por empresa.

### Operación productiva

- [ ] Feature flag considerada si hay riesgo.
- [ ] Idempotencia considerada en endpoints críticos.
- [ ] Queue reliability revisada para jobs.
- [ ] Audit trail aplicado en acciones sensibles.
- [ ] Deletion policy respetada.
- [ ] Error taxonomy aplicada.
- [ ] Documentation actualizada.
- [ ] Definition of Done cumplida.
