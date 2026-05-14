# Git Strategy Reference

## Propósito

Este documento define estrategia Git para cambios profesionales, revisables y seguros.

---

## Branching

Opciones válidas:

### Trunk-based

- Ramas cortas.
- PR pequeños.
- Feature flags.
- Integración frecuente.

### Git Flow ligero

- `main` producción.
- `develop` integración.
- `feature/*`.
- `release/*`.
- `hotfix/*`.

Usar la estrategia existente del proyecto.

---

## Commits semánticos

Formato recomendado:

```text
feat(orders): add idempotent order creation
fix(auth): prevent cross-company access
refactor(sync): split import service
chore(ci): add composer audit
```

Tipos:

- feat.
- fix.
- refactor.
- test.
- docs.
- chore.
- perf.
- security.

---

## Pull Requests

Un PR debe incluir:

- Objetivo.
- Cambios principales.
- Screenshots si UI.
- Tests.
- Riesgos.
- Rollback.
- Checklist security/performance.

---

## Squash rules

Preferir squash para ramas con commits ruidosos. Mantener commits separados si representan cambios lógicos independientes y útiles.

---

## Release tagging

Formato:

```text
v1.4.0
v1.4.1-hotfix.1
```

Tags deben asociarse a changelog.

---

## Hotfix strategy

1. Crear rama desde producción.
2. Aplicar cambio mínimo.
3. Ejecutar tests críticos.
4. Deploy.
5. Merge back a ramas activas.
6. Documentar incidente.

---

## Changelog

Mantener:

- Added.
- Changed.
- Fixed.
- Security.
- Deprecated.
- Removed.

---

## Checklist Git

- [ ] Rama correcta.
- [ ] Commits claros.
- [ ] PR pequeño.
- [ ] Tests documentados.
- [ ] Riesgos descritos.
- [ ] Rollback descrito.
- [ ] Changelog si aplica.
