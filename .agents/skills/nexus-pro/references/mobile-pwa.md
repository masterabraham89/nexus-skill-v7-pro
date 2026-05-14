# Mobile PWA Reference

## Propósito

Este documento define buenas prácticas para experiencia móvil y Progressive Web App dentro de NEXUS-PRO.

---

## Manifest

El archivo manifest debe definir:

- `name`.
- `short_name`.
- `description`.
- `start_url`.
- `display`.
- `background_color`.
- `theme_color`.
- Íconos en tamaños requeridos.
- Orientación si aplica.
- Categorías si aplica.

Ejemplo:

```json
{
  "name": "Mi App Enterprise",
  "short_name": "MiApp",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#111827"
}
```

---

## Service worker

Usos:

- Cache de assets.
- Offline fallback.
- Background Sync.
- Push notifications.
- Estrategias de revalidación.
- Limpieza de caches antiguos.

Reglas:

- Versionar caches.
- Invalidar correctamente.
- No cachear datos sensibles sin cifrado o justificación.
- Evitar servir información privada a otro usuario.
- Manejar logout limpiando caches privados.

---

## Offline first

Aplicar offline first cuando el negocio lo requiera:

- Formularios de campo.
- Captura de evidencias.
- Pedidos.
- Inventario.
- Visitas.
- Operación móvil con mala conectividad.

Reglas:

- Guardar operaciones pendientes.
- Mostrar estado pendiente.
- Sincronizar al recuperar conexión.
- Resolver conflictos.
- Evitar duplicados.
- Auditar sync.

---

## Background Sync

Usar para:

- Enviar formularios pendientes.
- Subir evidencias.
- Reintentar requests fallidos.
- Sincronizar catálogos.

Reglas:

- Idempotencia.
- Deduplicación.
- Retry con backoff.
- Límite de intentos.
- Registro de errores.
- UI de estado.

---

## IndexedDB

Usar para datos offline:

- Catálogos.
- Formularios pendientes.
- Evidencias temporales.
- Configuración local.
- Cola de sync.

Reglas:

- No guardar secretos.
- Limpiar datos al cerrar sesión.
- Versionar schema.
- Manejar migraciones.
- Cifrar si hay datos sensibles y el riesgo lo exige.

---

## Push notifications

Usar para:

- Alertas operativas.
- Estados de aprobación.
- Tareas asignadas.
- Recordatorios.
- Incidentes.

Reglas:

- Solicitar consentimiento.
- Permitir desactivar.
- No enviar datos sensibles en payload.
- Registrar preferencias.
- Respetar horarios si aplica.

---

## Cámara

Cuando se use cámara:

- Solicitar permisos claramente.
- Explicar propósito.
- Permitir reintento.
- Comprimir imagen.
- Validar peso y formato.
- Manejar errores de permisos.
- No acceder sin acción del usuario.

---

## Compresión de imágenes

Antes de subir:

- Redimensionar.
- Comprimir.
- Convertir a WebP si aplica.
- Limitar tamaño.
- Mantener calidad suficiente.
- Preservar metadata solo si es necesaria.
- Eliminar EXIF sensible cuando corresponda.

---

## UX móvil

Reglas:

- Botones grandes.
- Touch targets mínimos de 44x44 px.
- Formularios simples.
- Inputs adecuados por tipo.
- Feedback inmediato.
- Evitar tablas anchas.
- Usar cards o listas adaptativas.
- Mantener navegación clara.
- Evitar modales imposibles de cerrar.
- Probar en pantallas pequeñas.

---

## Touch targets

Elementos interactivos:

- Mínimo recomendado 44x44 px.
- Espaciado suficiente.
- No colocar acciones destructivas pegadas a acciones primarias.
- Confirmar acciones críticas.

---

## Checklist mobile/PWA

- [ ] Manifest configurado.
- [ ] Service worker versionado.
- [ ] Cache seguro.
- [ ] Offline state manejado.
- [ ] Background Sync idempotente.
- [ ] IndexedDB limpio en logout.
- [ ] Push con consentimiento.
- [ ] Cámara con permisos claros.
- [ ] Imágenes comprimidas.
- [ ] UX móvil revisada.
- [ ] Touch targets correctos.
