# Accessibility Reference

## Propósito

Este documento define los estándares de accesibilidad para interfaces desarrolladas con NEXUS-PRO. La meta mínima es cumplir WCAG 2.1 nivel AA cuando exista UI.

---

## WCAG 2.1 AA

Principios:

- Perceptible.
- Operable.
- Comprensible.
- Robusto.

Antigravity debe revisar accesibilidad en formularios, modales, tablas, botones, navegación, mensajes de error y estados dinámicos.

---

## Contraste

Reglas:

- Texto normal con contraste mínimo 4.5:1.
- Texto grande con contraste mínimo 3:1.
- No usar solo color para comunicar estado.
- Estados de error deben incluir texto o icono.
- Botones deshabilitados deben seguir siendo comprensibles.

---

## Labels

Todo input debe tener label visible o accesible.

Correcto:

```tsx
<label htmlFor="email">Correo electrónico</label>
<input id="email" name="email" type="email" />
```

Evitar placeholders como único label.

---

## Alt

Imágenes:

- Decorativas: `alt=""`.
- Informativas: alt descriptivo.
- Iconos con texto cercano: pueden ser decorativos.
- Imágenes de producto/documento: describir contenido útil.

---

## Teclado

Toda UI debe operar con teclado:

- Tab.
- Shift + Tab.
- Enter.
- Space.
- Escape para cerrar modales si aplica.
- Flechas en menús/listas cuando corresponda.

No debe existir trampa de foco.

---

## ARIA

Usar ARIA solo cuando HTML semántico no sea suficiente.

Buenas prácticas:

- `aria-label` para botones solo icono.
- `aria-describedby` para errores.
- `aria-expanded` en desplegables.
- `aria-controls` cuando aplique.
- `role="dialog"` en modales.
- `aria-modal="true"` en modales.

No usar ARIA para corregir HTML mal estructurado si puede resolverse con semántica nativa.

---

## Focus states

Todo elemento interactivo debe tener estado de foco visible.

Incluye:

- Botones.
- Links.
- Inputs.
- Selects.
- Checkboxes.
- Radio buttons.
- Tabs.
- Menús.

No eliminar outline sin reemplazo accesible.

---

## Errores accesibles

Los errores de formulario deben:

- Estar cerca del campo.
- Ser anunciables por lector de pantalla.
- Usar `aria-invalid`.
- Usar `aria-describedby`.
- Explicar cómo corregir.
- No depender solo de color.

Ejemplo:

```tsx
<input
  id="ruc"
  aria-invalid={Boolean(error)}
  aria-describedby={error ? "ruc-error" : undefined}
/>
{error && <p id="ruc-error">{error}</p>}
```

---

## axe-core

Usar axe-core para detectar:

- Labels faltantes.
- Contraste insuficiente.
- Roles inválidos.
- ARIA incorrecto.
- Jerarquía de headings.
- Elementos interactivos inaccesibles.

---

## Lighthouse

Usar Lighthouse para revisión general:

- Accessibility.
- Performance.
- Best practices.
- SEO si aplica.
- PWA si aplica.

No depender solo de Lighthouse. Hacer revisión manual.

---

## Modales accesibles

Deben:

- Mover foco al abrir.
- Devolver foco al cerrar.
- Tener título.
- Cerrar con Escape si corresponde.
- Bloquear foco dentro del modal.
- Tener overlay claro.
- No renderizarse como simple div sin semántica.

---

## Tablas accesibles

Deben:

- Usar `<table>` para datos tabulares.
- Usar `<th>`.
- Tener caption si aporta contexto.
- Evitar tablas para layout.
- Permitir navegación clara.
- Tener estados vacíos comprensibles.

---

## Checklist accessibility

- [ ] Contraste revisado.
- [ ] Inputs con labels.
- [ ] Imágenes con alt correcto.
- [ ] Navegación por teclado.
- [ ] Focus visible.
- [ ] Errores accesibles.
- [ ] ARIA correcto.
- [ ] Modales accesibles.
- [ ] axe-core considerado.
- [ ] Lighthouse revisado si aplica.
