# Frontend Design System Reference

## Propósito

Este documento define reglas de consistencia visual y UX para módulos React.

---

## Design tokens

Centralizar:

- Colores.
- Espaciado.
- Tipografía.
- Radios.
- Sombras.
- Z-index.
- Breakpoints.
- Estados.

No hardcodear valores repetidos en cada componente.

---

## Component variants

Componentes compartidos deben tener variantes controladas:

- Button: primary, secondary, danger, ghost.
- Badge: success, warning, error, neutral.
- Alert: info, success, warning, error.
- Input: default, error, disabled.

---

## Formularios

Todo formulario debe tener:

- Labels.
- Validación visible.
- Mensaje de error.
- Loading al enviar.
- Prevención doble submit.
- Confirmación si es destructivo.
- Accesibilidad.

---

## Tablas

Tablas deben tener:

- Loading skeleton o indicador.
- Empty state.
- Error state.
- Paginación.
- Sorting validado.
- Filtros claros.
- Responsive o vista mobile alternativa.

---

## Modales

Modales deben tener:

- Título.
- Descripción si aplica.
- Botón primario.
- Botón secundario.
- Cierre accesible.
- Focus management.
- Escape.
- Overlay.

---

## Empty states

Un empty state debe explicar:

- Qué no hay.
- Por qué puede estar vacío.
- Qué acción puede tomar el usuario.

---

## Dark mode

Si existe dark mode:

- Usar tokens.
- Revisar contraste.
- Evitar colores hardcodeados.
- Probar estados interactivos.

---

## Responsive rules

- Mobile first.
- Touch targets 44x44.
- Evitar tablas horizontales imposibles.
- Acciones principales visibles.
- Inputs adaptados.

---

## Checklist design system

- [ ] Usa tokens.
- [ ] Usa componentes compartidos.
- [ ] Estados UI completos.
- [ ] Responsive.
- [ ] Accesible.
- [ ] Sin estilos duplicados innecesarios.
