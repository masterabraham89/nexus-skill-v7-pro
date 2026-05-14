# Execution Policies

## Propósito

Este documento define límites operativos para Antigravity cuando programa usando NEXUS-4EVER. Su objetivo es evitar daños colaterales, refactors innecesarios, cambios silenciosos de contrato, pérdida de datos, migraciones peligrosas o decisiones no justificadas.

---

## Política 1: cambio mínimo seguro

Cuando el usuario pida corregir un bug, Antigravity debe:

1. Identificar causa raíz.
2. Cambiar la menor cantidad de archivos posible.
3. No refactorizar módulos completos si no fue solicitado.
4. No cambiar contratos públicos sin declararlo.
5. Agregar o sugerir prueba de regresión.

Prohibido:

- Reescribir pantallas completas por un bug local.
- Cambiar nombres de endpoints sin necesidad.
- Cambiar estructura de respuesta API sin documentar breaking change.
- Cambiar migraciones antiguas ya aplicadas en producción.

---

## Política 2: límite de alcance

Antes de modificar más de 5 archivos en una tarea simple, Antigravity debe justificar el alcance.

Clasificación:

- **Bugfix menor:** 1 a 3 archivos.
- **Feature pequeña:** 3 a 8 archivos.
- **Feature mediana:** 8 a 15 archivos.
- **Refactor grande:** requiere plan explícito.
- **Cambio transversal:** requiere checklist de regresión.

Si el cambio supera el alcance esperado, debe explicar por qué.

---

## Política 3: no romper contratos

Un contrato incluye:

- Rutas API.
- Payload request.
- Response JSON.
- Códigos de error.
- Eventos.
- Nombres de columnas expuestos.
- Props públicas de componentes.
- Formato de archivos import/export.

Cambiar un contrato requiere:

- Indicar breaking change.
- Proponer estrategia backward compatible.
- Actualizar OpenAPI si existe.
- Actualizar tests.
- Documentar migración para consumidores.

---

## Política 4: migraciones intocables

No modificar migraciones históricas que ya pudieron ejecutarse en producción. En su lugar:

- Crear nueva migración correctiva.
- Usar patrón expand/contract si hay cambios destructivos.
- Agregar columnas nullable primero si hay tráfico activo.
- Backfill por lotes.
- Eliminar columnas solo cuando ya no se usen.

---

## Política 5: dependencias nuevas

No agregar librerías nuevas sin justificar:

- Problema que resuelve.
- Alternativas nativas.
- Tamaño/peso.
- Mantenimiento del paquete.
- Licencia.
- Riesgos de seguridad.
- Impacto en bundle si es frontend.

---

## Política 6: seguridad bloqueante

Si un cambio introduce o mantiene una vulnerabilidad crítica, no debe entregarse como listo.

Bloqueantes:

- IDOR.
- SQL Injection.
- Mass Assignment crítico.
- Secrets expuestos.
- Auth bypass.
- XSS explotable.
- Endpoints privados sin autorización.
- Logs con tokens o contraseñas.
- `company_id` confiado desde cliente.

---

## Política 7: rollback primero

Para cambios críticos, responder internamente:

- ¿Cómo vuelvo atrás?
- ¿La migración tiene rollback seguro?
- ¿Hay feature flag?
- ¿Hay backup?
- ¿Qué pasa si el job falla a mitad?
- ¿Qué pasa si se ejecuta dos veces?

---

## Política 8: evidencia antes de optimizar

No optimizar por intuición si no hay señal. Priorizar:

- Query lenta medida.
- Render problemático observable.
- Bundle grande identificado.
- Endpoint con latencia alta.
- Job con timeout.
- N+1 evidente.

---

## Política 9: salida profesional

Toda entrega debe incluir:

- Qué se cambió.
- Por qué se cambió.
- Archivos afectados.
- Riesgos.
- Seguridad validada.
- Pruebas realizadas o recomendadas.
- Pendientes si existen.

---

## Checklist de ejecución

- [ ] Alcance controlado.
- [ ] Sin cambios silenciosos de contrato.
- [ ] Sin dependencias innecesarias.
- [ ] Sin migraciones históricas modificadas.
- [ ] Seguridad validada.
- [ ] Rollback considerado.
- [ ] Tests considerados.
- [ ] Entrega con evidencia.
