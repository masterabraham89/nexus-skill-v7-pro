# Performance Reference

## Propósito

Este documento define las prácticas de rendimiento para backend, frontend y PostgreSQL dentro de NEXUS-4EVER.

La performance debe evaluarse antes de entregar cambios que afecten listados, dashboards, reportes, sincronizaciones, imports, endpoints públicos, componentes con listas o flujos de alta frecuencia.

---

## Performance backend

### Reglas

- Evitar N+1.
- Usar eager loading específico.
- Paginar listados.
- No cargar datasets completos en memoria.
- Usar jobs para tareas pesadas.
- Usar cache con invalidación clara.
- Usar chunks de 200 para procesos masivos.
- Evitar loops con queries internas.
- Medir duración de procesos críticos.
- Registrar métricas de latencia.

---

## Performance frontend

### Reglas

- Code splitting por rutas pesadas.
- Lazy loading de componentes grandes.
- Evitar renders innecesarios.
- Virtualizar listas grandes.
- Debounce en búsquedas.
- Evitar cálculos pesados en render.
- Usar WebP/AVIF cuando aplique.
- Optimizar bundle size.
- Evitar librerías gigantes para tareas simples.
- Manejar cache con SWR/react-query.

---

## Performance PostgreSQL

### Reglas

- Revisar índices.
- Usar `EXPLAIN ANALYZE`.
- Activar análisis de queries lentas.
- Evitar `SELECT *`.
- Filtrar por columnas indexadas.
- Usar índices compuestos.
- Evitar offset profundo en tablas grandes.
- Considerar keyset pagination.
- Considerar materialized views para reportes.
- Considerar particionado para tablas enormes.

---

## Cache

### Usar cache para

- Catálogos.
- Configuraciones.
- Permisos relativamente estables.
- Reportes calculados.
- Consultas frecuentes de lectura.
- Datos públicos.

### No cachear sin cuidado

- Datos sensibles.
- Permisos sin invalidación.
- Saldos críticos.
- Estados transaccionales.
- Datos por usuario sin key segura.

### Reglas

- Definir TTL.
- Definir estrategia de invalidación.
- Incluir `company_id` en cache key cuando aplique.
- Evitar cache poisoning.
- No guardar secretos.

---

## Redis

Usos recomendados:

- Cache.
- Queues.
- Locks.
- Rate limiting.
- Sesiones si aplica.
- Deduplicación temporal.
- Control de concurrencia.

Ejemplo de lock:

```php
Cache::lock("sync-company-{$companyId}", 300)->block(5, function () {
    // sync seguro
});
```

---

## Lazy loading

Aplicar en:

- Módulos administrativos pesados.
- Reportes.
- Gráficos.
- Tablas avanzadas.
- Modales poco usados.
- Imágenes fuera del viewport.

---

## WebP

Para imágenes:

- Usar WebP o AVIF.
- Definir tamaños responsivos.
- Comprimir antes de subir si aplica.
- Lazy loading.
- Evitar imágenes enormes en mobile.
- Usar CDN si el tráfico lo justifica.

---

## Bundle size

Revisar:

- Librerías pesadas.
- Imports completos.
- Duplicación de dependencias.
- Componentes no usados.
- Moment.js u opciones grandes innecesarias.
- Gráficos incluidos globalmente.

Aplicar:

- Tree shaking.
- Dynamic imports.
- Bundle analyzer.
- División por rutas.

---

## Virtualización

Usar virtualización para:

- Tablas con cientos o miles de filas.
- Listados con scroll largo.
- Selectores masivos.
- Logs.
- Auditoría.
- Resultados de búsqueda extensos.

Opciones:

- `react-window`.
- `react-virtualized`.
- Virtualización propia si es simple.

---

## Compresión

En servidor/CDN:

- Gzip.
- Brotli.
- Compresión de assets.
- Cache headers.
- Minificación.
- HTTP/2 o HTTP/3 si la infraestructura lo soporta.

---

## HTTP/2

Beneficios:

- Multiplexing.
- Mejor carga de assets.
- Menor overhead.
- Mejor rendimiento en TLS.

Aun con HTTP/2, evitar assets innecesarios.

---

## Queries lentas

Cuando una query sea lenta:

1. Reproducir con datos similares a producción.
2. Ejecutar `EXPLAIN ANALYZE`.
3. Revisar índices.
4. Revisar filtros.
5. Evitar `SELECT *`.
6. Revisar joins.
7. Considerar materialized view.
8. Considerar cache.
9. Considerar particionado.
10. Documentar decisión.

---

## Jobs pesados

Mover a jobs:

- Imports.
- Exports.
- Envíos masivos.
- Reportes.
- Sync.
- Procesamiento de archivos.
- Cálculos masivos.
- Notificaciones.

Reglas:

- Chunks de 200.
- Idempotencia.
- Retry/backoff.
- Timeout razonable.
- Logs por lote.
- Métricas.
- Locks si hay concurrencia.

---

## Paginación

Obligatoria en listados.

Opciones:

- Offset pagination para tablas pequeñas/medianas.
- Cursor/keyset pagination para alto volumen.
- Filtros indexados.
- Límite máximo por página.

No permitir `per_page` ilimitado.

---

## ETag

Usar ETag en endpoints de lectura cacheables:

- Catálogos.
- Configuraciones.
- Recursos poco cambiantes.
- Assets generados.
- Reportes cerrados.

---

## Stale-while-revalidate

En frontend:

- SWR por defecto aplica esta mentalidad.
- Mostrar datos cacheados.
- Revalidar en background.
- Actualizar UI sin bloquear.
- Manejar errores sin destruir la experiencia.

---

## Checklist performance

- [ ] Listados paginados.
- [ ] Sin N+1.
- [ ] Índices considerados.
- [ ] Query crítica revisada.
- [ ] Jobs para procesos pesados.
- [ ] Chunks de 200.
- [ ] Cache considerada.
- [ ] Bundle size revisado.
- [ ] Lazy loading aplicado cuando corresponde.
- [ ] Imágenes optimizadas.
- [ ] Render performance revisado.
