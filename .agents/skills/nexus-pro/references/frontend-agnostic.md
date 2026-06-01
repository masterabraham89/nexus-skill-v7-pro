# Frontend Agnostic Reference

## Propósito

Este documento establece los principios arquitectónicos universales para el desarrollo de interfaces de usuario (frontend) dentro de NEXUS-PRO. El frontend debe diseñarse de forma modular, desacoplada del motor de API, con tipado estricto y preparada para adaptarse a distintos frameworks (React, Vue, Next.js, etc.).

---

## 🏗️ Arquitectura UI Agnóstica: Clean Frontend

Sin importar el framework de presentación, se debe mantener una separación clara de responsabilidades en tres capas fundamentales:

```text
Capa de Presentación (Componentes / JSX)
      ⬇️ (React Hooks / Vue Composables)
Capa de Control de Estado e Inyección (Stores / Context)
      ⬇️ (HTTP Services / Fetch Client)
Capa de Infraestructura y Transporte (API Callers / DTOs)
```

1. **Capa de Presentación (Componentes)**: Se limita puramente a renderizar elementos del DOM o UI y a capturar eventos del usuario. No conoce endpoints de red, query strings ni lógica de negocio compleja.
2. **Capa de Control (Hooks / Composables / ViewModels)**: Orquesta el estado local, maneja los ciclos de vida y enlaza los eventos de la presentación con la capa de infraestructura.
3. **Capa de Infraestructura (Services / SDKs)**: Ejecuta las peticiones HTTP directas al backend. Transforma las respuestas crudas de la API a interfaces tipadas del frontend.

---

## ⚡ Client State vs Server State

Una mala gestión del estado genera problemas de caché e inconsistencia. El frontend debe diferenciar:

### 1. Server State (Estado del Servidor)
- Datos provenientes de la base de datos (ej. lista de productos, órdenes, perfiles).
- **Regla**: No almacenar datos del servidor en variables de estado global del cliente (como Redux, Zustand o Pinia) para evitar la desincronización de caché.
- **Solución**: Usar librerías de caching y fetching dedicadas (`TanStack Query`, `useSWR`, `Vue Query`). Estas herramientas manejan de forma automática la rehidratación, expiración de caché y reintentos.

### 2. Client State (Estado del Cliente)
- Datos efímeros del navegador (ej. sidebar abierta/cerrada, filtros activos, temas visuales, pasos de un wizard).
- **Solución**: Usar manejadores de estado ligeros (`Zustand` para React, `Pinia` para Vue, o el estado local reactivo de cada componente).

---

## 🎨 Abstracción de Estilos y Temas

Para asegurar la coherencia estética en múltiples frameworks:
- **Tokens de Diseño**: Basar los colores, márgenes, fuentes y bordes en Variables CSS (`custom properties`) declaradas en un archivo de estilos global (`index.css`).
- Evitar utilidades CSS directas (ej. colores hardcodeados) en los componentes. Utilizar tokens semánticos (ej. `var(--color-primary-main)` en vez de `#3b82f6`).

---

## 📱 Mobile-First y Accesibilidad Touch

Para proyectos híbridos (Capacitor, Cordova) o PWAs:
- **Áreas de Contacto (Touch Targets)**: Todo elemento interactivo (botones, enlaces, filas de tabla clickeables) debe poseer un tamaño mínimo de **48x48px** o estar separado por márgenes que prevengan clics accidentales.
- **App Shell**: Estructurar la interfaz con cabeceras estáticas (`App Bar`) y menús de navegación inferiores (`Bottom Navigation`) en pantallas móviles, emulando la experiencia de una aplicación nativa.
- **Scroll Behavior**: Implementar scroll nativo suave y evitar el uso de scroll anidado complejo que rompa la experiencia táctil.

---

## 🩺 Checklist de Validación Frontend

Antes de finalizar una interfaz:

- [ ] La UI contempla los 5 estados críticos: Carga (loading), Error, Vacío (empty), Sin Permisos (unauthorized) y Éxito.
- [ ] No existen llaves de APIs privadas ni configuraciones sensibles expuestas en el código fuente final.
- [ ] Se gestionó la limpieza de eventos (`useEffect` cleanup / `onUnmounted` en Vue) en listeners globales o websockets.
- [ ] El diseño es responsive y se adapta correctamente a dispositivos móviles de distintas resoluciones.
- [ ] Las imágenes y recursos pesados cargan bajo demanda (lazy loading).
- [ ] Las interacciones y formularios cuentan con validación visual en tiempo real.
