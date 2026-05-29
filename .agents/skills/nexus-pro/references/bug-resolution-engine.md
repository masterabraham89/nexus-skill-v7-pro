# Bug Resolution Engine (BRE) — NEXUS-PRO v7

## Propósito

Este documento es el **motor operativo de resolución de bugs de NEXUS-PRO**. Define el protocolo completo que Antigravity debe seguir, de forma autónoma y sin adivinar, cuando recibe un reporte de error. No es solo una guía de diagnóstico — es un **sistema de instrucciones de alto nivel** que convierte a Antigravity en un ingeniero SRE senior que detecta, clasifica, analiza la causa raíz, propone y verifica la solución estructural.

---

## FASE 0 — Activación del Motor

Antigravity DEBE activar este motor cuando el usuario envíe cualquiera de las siguientes señales:

- Un mensaje de error del servidor (stack trace, error 500, error 422, etc.)
- Una captura de pantalla de un bug visual o de consola
- Una descripción de comportamiento inesperado ("no guarda", "se rompe", "no carga")
- Un log de red del navegador (DevTools → Network)
- Una excepción de PHP/Laravel en los logs del servidor
- Un error de TypeScript o consola del navegador

**Al activarse, la IA DEBE DETENER cualquier tarea en curso y entrar en Modo BRE.**

---

## FASE 1 — Triage y Clasificación (< 30 segundos)

Antes de hacer NADA más, Antigravity debe clasificar el bug usando la siguiente tabla cruzada. Esta clasificación determina la urgencia, el tono de respuesta y el protocolo a seguir.

### Tabla de Clasificación NEXUS-BRE

| Nivel | Categoría | Severidad NEXUS | P-Level | Síntoma Típico | Acción Inmediata |
|---|---|---|---|---|---|
| 1 | **Crítico (Blocker)** | MÁXIMA | P1 | App no arranca, login roto, caída total, pérdida de datos | CONTENCIÓN + ROLLBACK |
| 2 | **Mayor** | ALTA | P2 | Módulo core roto (pagos, facturas), datos corruptos intermitentes | HOTFIX + TEST DE REGRESIÓN |
| 3 | **Menor** | MEDIA | P3 | Textos incorrectos, lógica secundaria rota, validaciones erróneas | ANÁLISIS ESTRUCTURAL + FIX |
| 4 | **Cosmético / UI** | BAJA | P4 | Desalineación, iconos mal posicionados, colores incorrectos | FIX DE ESTILOS + DESIGN TOKEN |
| 5 | **Rendimiento** | MEDIA-ALTA | P2/P3 | Carga lenta, RAM alta, queries lentas, N+1 | PROFILING + OPTIMIZACIÓN |
| 6 | **Seguridad** | MÁXIMA | P1 | IDOR, SQL Injection, token expuesto, datos de otro tenant | CONTENCIÓN INMEDIATA + PARCHE ESTRUCTURAL |
| 7 | **Compatibilidad** | BAJA-MEDIA | P3/P4 | Falla en Safari/Firefox/móvil, resolución específica rota | AISLAMIENTO DE ENTORNO + POLYFILL |

### Preguntas de Triage Obligatorias

Al recibir el reporte, antes de proponer cualquier solución, Antigravity DEBE responder internamente:

1. ¿Está ocurriendo en **Producción** o en **Desarrollo**?
2. ¿Cuántos usuarios están impactados? (1 usuario, grupo específico, todos)
3. ¿Es un error **nuevo** o ya existía antes?
4. ¿Existe un **commit reciente** o deploy que pueda haber introducido el bug?

---

## FASE 2 — Protocolo de Respuesta por Nivel

### 🔴 P1 — Crítico y Seguridad (Severidad MÁXIMA)

**Comportamiento mandatorio de Antigravity:**

```
ESTADO: MODO EMERGENCIA — Sin charla, sin introducciones largas.
```

1. **CONTENCIÓN INMEDIATA:** Sugerir rollback de git, desactivar Feature Flag o bloquear el endpoint afectado.
2. **IDENTIFICACIÓN DEL EPICENTRO:** Localizar el commit o línea de código causante usando `git log --oneline -10` o búsqueda en logs de servidor.
3. **HOTFIX QUIRÚRGICO:** Aplicar el cambio mínimo necesario para restaurar el sistema. No refactorizar en caliente durante P1.
4. **VERIFICACIÓN:** Confirmar que el sistema responde con HTTP 200 / comportamiento esperado.
5. **POST-MORTEM:** Después de estabilizar, generar el análisis de causa raíz completo con la Metodología de los 5 Porqués.
6. **ADR:** Generar un Architecture Decision Record en `docs/architecture/decisions/` documentando qué ocurrió y qué cambio estructural previene que vuelva a ocurrir.

**Para Bugs de Seguridad (P1 especial):**
- Cerrar la brecha primero (política, middleware o validación de ownership).
- Nunca exponer el detalle de la vulnerabilidad en el mensaje de respuesta del API.
- Verificar que `companyId` se extrae del token, no del cliente.
- Confirmar que todas las rutas afectadas tienen Policy/Gate correctamente aplicado.

---

### 🟠 P2 — Mayor / Rendimiento Grave (Severidad ALTA)

**Comportamiento mandatorio de Antigravity:**

1. **TRIANGULACIÓN OBLIGATORIA (3 capas):**
   - Capa 1 (Entrada): ¿Qué envía el frontend? Verificar payload de Network → Request Body.
   - Capa 2 (Lógica): ¿Qué valida el FormRequest y qué hace el Service? Leer código real.
   - Capa 3 (Persistencia): ¿Qué tiene la migración y la tabla en la DB? Confirmar con `schema.sql` o migration.

2. **HIPÓTESIS ESTRUCTURADA:** Generar exactamente 3 hipótesis clasificadas por capa antes de proponer código:
   - **Hipótesis A (Datos):** ¿Falta columna? ¿Tipo incompatible? ¿Constraint violado? ¿Race condition?
   - **Hipótesis B (Lógica/Red):** ¿Payload no coincide con DTO? ¿Timeout? ¿Estado de sincronización roto?
   - **Hipótesis C (Infra/Permisos):** ¿Rol sin acceso? ¿CORS? ¿Variable de entorno faltante?

3. **SOLUCIÓN ESTRUCTURAL + IDEMPOTENCIA:** Para operaciones de pago o acciones destructivas, verificar que existe `X-Idempotency-Key` y que el endpoint no puede ejecutarse dos veces con efecto doble.

4. **CIRCUIT BREAKER (si aplica frontend):** Si el servicio falla repetidamente, añadir patrón Circuit Breaker para evitar cascada de peticiones fallidas al backend.

5. **TEST DE REGRESIÓN:** Antes de cerrar el ticket, escribir el test que reproduce el bug para prevenir regresión.

---

### 🟡 P3 — Menor / Compatibilidad (Severidad MEDIA)

**Comportamiento mandatorio de Antigravity:**

1. **AISLAMIENTO DEL COMPONENTE AFECTADO:** Identificar el archivo exacto (componente, hook, servicio backend) sin explorar el proyecto entero.

2. **CAUSA RAÍZ SIMPLE:** Para textos incorrectos → verificar si hay hardcoding en el JSX o si falta una clave en el archivo de traducciones i18n. Para lógica secundaria rota → trazar el hook o el service correspondiente.

3. **CORRECCIÓN MÍNIMA (Scope Control):** Modificar únicamente lo necesario. Si se detecta deuda técnica adicional durante la revisión, reportarla como recomendación separada, NO tocarla en este ticket.

4. **VERIFICACIÓN DE ESTADOS UI:** Confirmar que el componente maneja correctamente los estados: `loading`, `error`, `empty`, `success`.

---

### 🔵 P4 — Cosmético / UI (Severidad BAJA)

**Comportamiento mandatorio de Antigravity:**

1. **RESPETAR EL DESIGN SYSTEM:** Usar únicamente tokens de espaciado, color y tipografía definidos en el sistema de diseño. Prohibido añadir valores CSS arbitrarios.

2. **VERIFICACIÓN MULTI-RESOLUCIÓN:** Confirmar que el fix se ve correctamente en: 375px (móvil), 768px (tablet), 1280px (escritorio).

3. **ACCESSIBILITY CHECK:** Verificar que el fix no rompe contraste mínimo WCAG 2.1 AA (4.5:1 para texto normal).

4. **NO CREAR DEUDA CSS:** Si el bug cosmético existe porque hay estilos inline o clases duplicadas, limpiar y centralizar en el token correspondiente.

---

## FASE 3 — Metodología Universal de Resolución (5 Pasos)

Esta metodología aplica a TODOS los niveles de severidad. Es el núcleo del BRE.

```
┌─────────────────────────────────────────────────────────────────┐
│            NEXUS BRE — CICLO DE RESOLUCIÓN UNIVERSAL            │
└─────────────────────────────────────────────────────────────────┘

 PASO 1: REPRODUCIR → PASO 2: AISLAR → PASO 3: DIAGNOSTICAR
       → PASO 4: RESOLVER → PASO 5: VERIFICAR + BLINDAR
```

### PASO 1 — REPRODUCIR

Antes de tocar cualquier código, Antigravity debe poder describir con exactitud:

- ¿En qué condiciones exactas ocurre el error?
- ¿Es reproducible consistentemente o intermitente?
- ¿Hay una secuencia de pasos específica para reproducirlo?

**Si no se puede reproducir, se DETIENE y se pide más información al usuario.**

### PASO 2 — AISLAR (Máximo 3 lecturas de archivo)

Usando las herramientas disponibles (`view_file`, `grep_search`), trazar la ruta del error de forma sistemática:

```
Backend:  Route → FormRequest → Controller → Service → Repository → DB
Frontend: Page → Component → Hook → Service → API → Response
```

**Regla de los 3 Pasos:** Si tras 3 lecturas de archivo no se localiza el error, DETENER y pedir al usuario el archivo exacto o el log completo. Prohibido explorar el proyecto aleatoriamente.

### PASO 3 — DIAGNOSTICAR (Los 5 Porqués)

Aplicar la metodología de los 5 Porqués antes de escribir código:

```
Ejemplo:
├── Síntoma:   "El endpoint de pagos devuelve error 500"
├── ¿Por qué? → Porque la DB rechaza la escritura
├── ¿Por qué? → Porque viola una constraint UNIQUE
├── ¿Por qué? → Porque se está intentando insertar el mismo transaction_id dos veces
├── ¿Por qué? → Porque el frontend hace doble submit al hacer doble clic
└── Causa Raíz: Falta mecanismo de idempotencia + debounce en el botón
```

**Antigravity DEBE documentar esta cadena de razonamiento antes de proponer código.**

### PASO 4 — RESOLVER (Solución Estructural, No Parche)

La solución debe atacar la **causa raíz**, no el síntoma. Verificar que la solución propuesta:

- ✅ Sigue la arquitectura limpia (Controller → Service → Repository).
- ✅ No viola el principio de responsabilidad única.
- ✅ No crea un archivo que supere los límites de líneas del Anti-Patch Policy.
- ✅ No introduce hardcoding, secretos en código o `any` sin justificación.
- ✅ Incluye manejo de errores explícito con el formato estándar de la Error Taxonomy.
- ✅ Respeta el aislamiento multi-tenant (si aplica).

### PASO 5 — VERIFICAR + BLINDAR (Test-Driven Repair)

Después de aplicar la corrección, el ciclo NO está completo hasta que se ejecute:

1. **Test de Reproducción (Red):** Escribir un test unitario o de integración que demuestre el bug ANTES del fix.
2. **Test de Corrección (Green):** Verificar que el mismo test pasa DESPUÉS del fix.
3. **Test de Regresión:** Asegurarse de que los tests existentes del módulo siguen pasando.
4. **Blast Radius Check:** Usar `grep_search` para identificar si otros módulos consumen la función o endpoint corregido y confirmar que no se introdujo una regresión.

---

## FASE 4 — Guías de Solución por Tipo de Bug

### 🔴 Bug Crítico — Guía de Resolución

| Síntoma | Causa Raíz Típica | Solución Estructural |
|---|---|---|
| App no inicia / pantalla blanca | Error en Provider/Router inicial, variable de entorno faltante | Revisar `console.error` en navegador, verificar `.env`, verificar rutas en `app/Providers` |
| Login roto (redirect loop) | Middleware de autenticación mal configurado o token corrupto | Revisar `auth:sanctum` middleware, limpiar cache de Sanctum, verificar `SESSION_DOMAIN` |
| Error 500 en producción | Excepción no manejada, migración no ejecutada, Redis caído | Revisar `storage/logs/laravel.log`, ejecutar `php artisan migrate --force`, verificar conexión Redis |
| Pérdida de datos en sync | Race condition en upsert sin lock | Implementar `DB::transaction()` con `lockForUpdate()` en el Repository |

---

### 🟠 Bug Mayor — Guía de Resolución

| Síntoma | Causa Raíz Típica | Solución Estructural |
|---|---|---|
| No se puede completar un pago | Double-submit, Unique Constraint, timeout de provider externo | Idempotency Key (`X-Idempotency-Key`), debounce en botón frontend, Circuit Breaker |
| Importación masiva falla a mitad | Chunk no implementado, tiempo de ejecución agotado | Upsert en chunks de 200, mover a Job asíncrono en Queue, implementar retry con backoff |
| Datos de otro tenant visibles | `companyId` recibido del cliente, Policy no aplicada | Extraer `companyId` del `$request->user()->company_id`, añadir `->where('company_id', $companyId)` en Repository |
| N+1 queries en listado | Eager loading faltante | Añadir `->with(['relation1', 'relation2'])` en Repository, verificar con `DB::enableQueryLog()` |

---

### 🟡 Bug Menor — Guía de Resolución

| Síntoma | Causa Raíz Típica | Solución Estructural |
|---|---|---|
| Texto incorrecto en UI | String hardcodeado en JSX | Mover texto a `/locales/es.json` y consumir con hook de i18n |
| Validación incorrecta en formulario | Regla de validación mal definida o ausente en FormRequest | Revisar `rules()` en el FormRequest correspondiente, añadir regla específica |
| Estado de carga no aparece | `isLoading` no conectado al componente | Verificar que el hook expone `isLoading` y el componente lo consume para renderizar skeleton/spinner |
| Modal no cierra después de guardar | Llamada a `onClose()` faltante en el `onSuccess` del hook | Añadir `onClose()` en el callback de éxito del hook o mutation |

---

### 🔵 Bug Cosmético — Guía de Resolución

| Síntoma | Causa Raíz Típica | Solución Estructural |
|---|---|---|
| Elemento desbordado en móvil | `width: px` fijo sin `max-width` | Usar `max-width: 100%`, `overflow: hidden` o Flexbox/Grid responsivo |
| Ícono desalineado | `display: inline` mezclado con padding | Cambiar a `display: flex; align-items: center` en el contenedor |
| Colores inconsistentes | Valor hex hardcodeado vs variable del design system | Reemplazar por token del design system (ej. `var(--color-primary-500)`) |
| Texto truncado inesperadamente | `white-space: nowrap` sin `text-overflow` definido | Añadir `text-overflow: ellipsis; overflow: hidden` o eliminar `nowrap` |

---

### ⚡ Bug de Rendimiento — Guía de Resolución

| Síntoma | Causa Raíz Típica | Solución Estructural |
|---|---|---|
| Página lenta en carga inicial | Bundle JS demasiado grande | Implementar `React.lazy()` + `Suspense` para code splitting por módulo |
| Query tarda +3 segundos | Falta índice en columna de filtro o `SELECT *` | Añadir `$table->index(['company_id', 'status'])` en migración, usar `select([...columnas])` |
| Componente hace re-render excesivo | Estado global innecesario o contexto sin memoización | Envolver en `useMemo`/`useCallback` con dependencias correctas, revisar si el estado pertenece al contexto o al componente local |
| `useEffect` causa memory leak | Falta cleanup / AbortController | Añadir función de cleanup: `return () => { subscription.unsubscribe(); }` |
| RAM alta por listado grande | Array completo en estado, sin paginación virtual | Implementar paginación del backend (cursor-based o offset), usar `react-virtual` para listas de >500 ítems |

---

### 🔒 Bug de Seguridad — Guía de Resolución

| Síntoma | Causa Raíz Típica | Solución Estructural |
|---|---|---|
| Usuario A ve datos de Usuario B (IDOR) | Policy no aplicada, `find()` sin filtro de `company_id` | Reemplazar `Model::find($id)` por `Model::where('company_id', $companyId)->findOrFail($id)` |
| SQL Injection posible | Uso de `DB::raw()` con interpolación de string | Reemplazar por bindings preparados: `DB::raw('column = ?', [$value])` o usar Query Builder |
| Token o API key en código fuente | Hardcoding de secretos | Mover a `.env`, verificar con `grep_search 'API_KEY'` que no existan en código, añadir a `.gitignore` |
| Endpoint sin rate limit | Ruta pública sin middleware `throttle` | Añadir `->middleware('throttle:60,1')` en `routes/api.php` para la ruta afectada |
| Log expone contraseña o token | `Log::info(['password' => $password])` | Implementar sanitización: nunca loggear campos sensibles; usar allowlist de campos seguros |

---

### 🌐 Bug de Compatibilidad — Guía de Resolución

| Síntoma | Causa Raíz Típica | Solución Estructural |
|---|---|---|
| Animación rota en Safari | Uso de API CSS no soportada | Verificar en caniuse.com, añadir prefijo `-webkit-` o usar alternativa con soporte amplio |
| Fetch falla en iOS 15 o anterior | API moderna no soportada (ej. `AbortController` sin polyfill) | Añadir polyfill específico en `vite.config.ts` usando `@vitejs/plugin-legacy` |
| Layout roto en pantallas <375px | Media query no definida para móvil pequeño | Añadir `@media (max-width: 375px)` con ajustes específicos |
| Selector CSS ignorado en Firefox | Selector CSS propietario de Chrome | Reemplazar por selector estándar compatible con todos los motores |

---

## FASE 5 — Checklist de Cierre (Definition of Done para Bugs)

Antigravity NO puede declarar un bug como resuelto hasta que pueda confirmar TODOS los ítems de esta lista:

```markdown
### ✅ Checklist de Cierre NEXUS-BRE

**Diagnóstico**
- [ ] Bug clasificado por nivel (1-7) y P-Level (P1-P4)
- [ ] Causa raíz estructural identificada (no solo el síntoma)
- [ ] Metodología 5 Porqués documentada (al menos 3 niveles de profundidad)
- [ ] Se verificó con herramientas (no se adivinó la solución)

**Solución**
- [ ] Corrección ataca la causa raíz, no el síntoma
- [ ] Arquitectura respetada (Controller → Service → Repository / Page → Hook → Service)
- [ ] Sin hardcoding de secretos o valores arbitrarios
- [ ] Sin violación de límites de líneas por archivo (Anti-Patch Policy)
- [ ] Error Taxonomy aplicada: respuesta con `code`, `message`, `request_id`
- [ ] Ownership validado si el fix toca datos de empresa/usuario
- [ ] Sin regresión de seguridad (IDOR, Mass Assignment, Rate Limit)

**Verificación**
- [ ] Test de reproducción escrito (Red → Green con Test-Driven Repair)
- [ ] Blast Radius revisado: otros módulos que usan el código corregido siguen funcionando
- [ ] Estados UI verificados: loading, error, empty, success
- [ ] Verificado en resolución mobile/tablet/desktop (para bugs UI)
- [ ] Sin nuevos warnings en consola del navegador o logs del servidor

**Cierre**
- [ ] ADR generado si el fix introduce un cambio arquitectónico
- [ ] `.nexus-memory.md` actualizado si el bug revela un patrón recurrente
- [ ] Commit con formato Conventional Commits: `fix(módulo): descripción clara`
```

---

## Regla Final del BRE

> **El Bug Resolution Engine de NEXUS-PRO no existe para escribir código rápido. Existe para escribir el código correcto la primera vez.**

Si Antigravity enfrenta una situación donde resolver el bug requiere violar las reglas de arquitectura, seguridad o scope del proyecto, DEBE informar al usuario y proponer la solución correcta aunque sea más lenta de implementar. **Nunca se prioriza la velocidad sobre la integridad del sistema.**
