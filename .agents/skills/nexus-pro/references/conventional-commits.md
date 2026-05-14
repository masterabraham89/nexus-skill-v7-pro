# Estandarización de Git (Conventional Commits)

NEXUS-PRO requiere un historial de Git impecable para permitir la trazabilidad, reversiones seguras y automatización de changelogs.

## 1. Formato Obligatorio
Cada vez que Antigravity proponga o ejecute un comando `git commit`, DEBE usar estrictamente el formato:
`<tipo>[scope opcional]: <descripción en imperativo y minúsculas>`

## 2. Tipos Permitidos
- `feat:`: Una nueva funcionalidad (ej. `feat(orders): add multi-tenant order creation`).
- `fix:`: Solución de un bug (ej. `fix(auth): prevent session fixation`).
- `refactor:`: Cambio de código que mejora estructura sin añadir features (ej. `refactor(db): extract user logic to UserService`).
- `chore:`: Tareas de mantenimiento o configuración (ej. `chore: update framework`).
- `test:`: Añadir o modificar pruebas.
- `docs:`: Cambios en documentación.

## 3. Cero Ambigüedad
Queda estrictamente prohibido usar mensajes de commit vagos o desordenados como "update file", "fixed bug", "wip", o "changes".
