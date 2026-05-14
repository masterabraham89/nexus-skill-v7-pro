# UX Failure States Reference

## Propósito

Este documento amplía los estados UI obligatorios. Una interfaz robusta no solo muestra loading/error/empty; debe manejar permisos, sesión, conexión, conflictos y recuperación.

## Estados obligatorios

### Loading

- Skeleton o mensaje claro.
- No bloquear toda la pantalla si solo carga una sección.

### Error

- Mensaje claro.
- Acción de retry cuando aplique.
- request_id si sirve para soporte.

### Empty

- Explicar que no hay datos.
- Ofrecer acción siguiente.

### Sin permisos

- Mensaje respetuoso.
- No mostrar acciones bloqueadas.
- Backend siempre valida.

### Offline

- Indicar sin conexión.
- Mostrar datos cacheados si existen.
- Permitir guardar pendiente si PWA lo soporta.

### Sesión expirada

- Redirigir a login o mostrar modal.
- No perder datos de formulario si es posible.

### Datos parciales

- Indicar que la información puede estar incompleta.
- Permitir recargar.

### Retry

- Mostrar intento de reconexión.
- Evitar loops infinitos.

### Conflicto

- Explicar que el dato cambió.
- Permitir recargar, sobrescribir solo si autorizado o comparar.

### Mantenimiento

- Mensaje claro.
- Evitar errores crudos.

### Acción irreversible

- Confirmación explícita.
- Explicar consecuencia.
- Requerir permiso.

## Checklist

- [ ] Loading.
- [ ] Error.
- [ ] Empty.
- [ ] Sin permisos.
- [ ] Offline si aplica.
- [ ] Sesión expirada.
- [ ] Retry.
- [ ] Conflicto.
- [ ] Acción irreversible confirmada.
- [ ] Mensajes claros y accesibles.
