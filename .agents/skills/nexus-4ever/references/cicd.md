# CI/CD Reference

## Propósito

Este documento define el pipeline mínimo recomendado para NEXUS-4EVER. Todo despliegue debe ser repetible, auditable y reversible.

---

## Objetivos del pipeline

- Detectar errores antes de producción.
- Bloquear código inseguro.
- Ejecutar pruebas.
- Validar tipos.
- Validar estilo.
- Construir assets.
- Proteger secretos.
- Permitir rollback.
- Verificar salud del sistema.

---

## Etapas recomendadas

```text
checkout
  -> install dependencies
  -> lint
  -> type check
  -> static analysis
  -> tests
  -> security audit
  -> build
  -> deploy staging
  -> smoke tests
  -> deploy production
  -> health checks
```

---

## Lint

Backend:

- PHP CS Fixer o Laravel Pint.
- Reglas consistentes por proyecto.

Frontend:

- ESLint.
- Reglas TypeScript.
- Reglas React Hooks.
- No variables sin usar.
- No imports muertos.

---

## Type check

Frontend:

```bash
npm run typecheck
```

Debe bloquear:

- Tipos incompatibles.
- Props incompletas.
- Responses no tipadas.
- Uso peligroso de `any`.

---

## PHPStan

Usar PHPStan o Larastan.

Debe revisar:

- Tipos.
- Métodos inexistentes.
- Contratos rotos.
- Nullability.
- Errores de acceso.
- Inconsistencias en services/repositories.

---

## ESLint

Debe validar:

- Reglas React.
- Reglas hooks.
- Imports.
- Variables sin uso.
- Dependencias de `useEffect`.
- Uso peligroso de `any`.
- Accesibilidad básica si se configura.

---

## Prettier

Debe formatear:

- TypeScript.
- TSX.
- JSON.
- Markdown.
- CSS.

No usar Prettier para ocultar problemas de arquitectura.

---

## npm audit

Ejecutar:

```bash
npm audit
```

Reglas:

- Bloquear vulnerabilidades críticas.
- Revisar vulnerabilidades altas.
- Documentar excepciones temporales.
- Actualizar dependencias con cuidado.

---

## composer audit

Ejecutar:

```bash
composer audit
```

Reglas:

- Bloquear vulnerabilidades críticas.
- Revisar paquetes Laravel/PHP.
- Actualizar dependencias de seguridad.
- No ignorar CVEs sin justificación.

---

## Build

Frontend:

```bash
npm run build
```

Backend:

- Composer install optimizado.
- Cache config/routes/views según entorno.
- Migraciones controladas.
- Assets versionados.

---

## Staging

Todo cambio relevante debe pasar por staging si afecta:

- Auth.
- Roles.
- Datos sensibles.
- Pagos.
- Imports.
- Sync.
- Reportes.
- Migraciones.
- Performance.
- Jobs.
- Infraestructura.

---

## Production

Antes de producción:

- Backup reciente.
- Migraciones revisadas.
- Plan de rollback.
- Health checks.
- Variables de entorno verificadas.
- Workers activos.
- Cola monitoreada.
- Logs disponibles.
- Alertas activas.

---

## Branch protection

Configurar:

- Pull request obligatorio.
- Reviews requeridos.
- Pipeline exitoso.
- Bloqueo de force push.
- Bloqueo de push directo a main.
- Reglas para ramas release.
- Firma de commits si aplica.

---

## Rollback

Debe existir plan de rollback:

- Revert de release.
- Rollback de contenedor/build.
- Rollback de migraciones cuando sea posible.
- Feature flags para apagar funcionalidades.
- Backups si se afecta estructura crítica.
- Runbook documentado.

---

## Health checks

Validar después del deploy:

- API responde.
- Login funciona.
- DB disponible.
- Redis disponible.
- Workers procesan.
- Queue sin acumulación crítica.
- Sentry sin errores nuevos.
- Latencia dentro de umbral.
- Endpoint `/health` o equivalente.

---

## Secrets en CI/CD

Reglas:

- Usar secrets manager del proveedor CI/CD.
- No imprimir secretos.
- No guardar `.env` en repo.
- No exponer tokens en logs.
- Separar secrets por entorno.
- Rotar credenciales.
- Usar permisos mínimos en tokens de deploy.

---

## Checklist CI/CD

- [ ] Lint backend ejecutado.
- [ ] Lint frontend ejecutado.
- [ ] Typecheck ejecutado.
- [ ] PHPStan/Larastan ejecutado.
- [ ] Tests ejecutados.
- [ ] npm audit revisado.
- [ ] composer audit revisado.
- [ ] Build exitoso.
- [ ] Secrets protegidos.
- [ ] Staging validado.
- [ ] Rollback preparado.
- [ ] Health checks ejecutados.
