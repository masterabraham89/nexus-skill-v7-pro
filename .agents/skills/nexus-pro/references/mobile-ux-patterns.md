# Mobile UX Patterns (Flutter-Style Architecture)

Este documento es el cerebro de diseño móvil de **NEXUS-PRO**. Antigravity DEBE consultar y aplicar estas reglas siempre que el usuario solicite un diseño frontend, una vista web, una PWA o una funcionalidad móvil, con el objetivo de estructurar el UI tal como lo haría un desarrollador experto en Flutter (Material/Cupertino).

## 📱 1. El "Scaffold" (App Shell Container)
En móvil/PWA, las aplicaciones **nunca** deben hacer scroll incontrolado en `body` ni expandirse a todo el ancho en monitores grandes.
- **Regla:** Envolver siempre la vista en un contenedor restringido.
- **Implementación React/Tailwind:**
  ```tsx
  // El equivalente al Scaffold de Flutter
  <div className="w-full max-w-md mx-auto h-[100dvh] flex flex-col bg-gray-50 shadow-xl overflow-hidden relative">
      {/* 1. AppBar (Sticky Header) */}
      {/* 2. Body (Flex-1 + Overflow-y-auto) */}
      {/* 3. BottomNavigationBar / FloatingActionButton */}
  </div>
  ```

## 📐 2. Anatomía por Plataforma (Arquetipos)

### A. E-commerce / Catálogos
- **Grilla de Productos:** `grid grid-cols-2 gap-3 p-4`. Imágenes 1:1 o 4:5. Botones de "Añadir" pequeños pero clickeables (44x44px mínimo).
- **Detalle de Producto:** Carrusel horizontal con *snap* (`flex overflow-x-auto snap-x snap-mandatory`).
- **Botón de Compra Fijo:** Siempre fijar el total y botón de "Pagar" en la parte inferior de la pantalla (`fixed bottom-0 w-full max-w-md pb-safe`).

### B. POS (Punto de Venta)
- **Categorías Superiores:** Barra horizontal deslizable para filtros/categorías (`overflow-x-auto whitespace-nowrap`). Evitar menús desplegables.
- **Acciones Rápidas:** Los botones para agregar al ticket deben ser grandes y fáciles de presionar sin mirar.
- **Carrito en Bottom Sheet:** El resumen de la venta no se muestra al lado derecho (como en Desktop), sino en un Bottom Sheet (cajón inferior) que se arrastra hacia arriba.

### C. Paneles Administrativos (Dashboards)
- **Bottom Navigation Bar:** Prohibido usar Sidebar izquierdo en móvil. Usar una barra de navegación en la parte inferior con 3-5 iconos (Home, Reportes, Ajustes, etc.).
- **Data Cards:** Transformar tablas pesadas en `cards` apilables verticalmente. Si una tabla es indispensable, ponerla dentro de un `<div className="overflow-x-auto w-full">`.
- **Filtros Ocultos:** Botón "Filtros" que abra un *Full Screen Dialog* o un *Bottom Sheet*.

### D. Formularios y Landing Pages (Wizards)
- **Wizards Paso a Paso:** Formularios de más de 6 campos se dividen en pasos.
- **Sticky CTA:** El botón "Guardar" o "Continuar" siempre fijo abajo (`mt-auto` si es flex-col, o `fixed bottom-0`).
- **Inputs Ergonométricos:** Altura mínima de input `h-12` (48px). Tamaño de fuente `text-base` (16px) para evitar que iOS haga zoom automático.

## 🖐️ 3. Reglas de Ergonomía y Touch (Thumb Zone)
- **Botones nunca gigantes:** Evitar que un botón ocupe toda la pantalla. Si usa ancho completo, que sea `w-full max-w-sm mx-auto`.
- **Áreas Táctiles:** Cualquier elemento interactivo debe tener mínimo `44px` x `44px` de área clickeable.
- **Bottom Sheets > Modals:** Quedan PROHIBIDOS los modales clásicos flotando al centro de la pantalla en vista móvil. Todo diálogo o alerta debe surgir desde abajo (Bottom Sheet).

## 🛡️ 4. Safe Areas y Micro-Interacciones
- **Notch y Barras Home:** Siempre usar clases para el padding seguro del sistema (e ej. en Tailwind usando un plugin o variables `pt-[env(safe-area-inset-top)] pb-[env(safe-area-inset-bottom)]`).
- **Feedback visual:** Botones deben tener estado `active:scale-95` o background distinto al ser presionados (efecto Ripple de Flutter).

> **Alerta para Antigravity:** Al generar código de componentes, PIENSA EN MÓVIL PRIMERO (Mobile-First). No entregues componentes que se rompan en pantallas de 375px de ancho.
