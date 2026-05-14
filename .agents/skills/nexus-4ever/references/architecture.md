# Architecture Reference

## Propósito

Este documento define la arquitectura obligatoria para proyectos desarrollados con NEXUS-4EVER. La arquitectura debe favorecer separación de responsabilidades, bajo acoplamiento, alta cohesión, seguridad, mantenibilidad y escalabilidad.

El stack principal es:

- Backend: Laravel / PHP.
- Frontend: React / TypeScript.
- Base de datos: PostgreSQL.
- Arquitectura backend: `Controller -> Service -> Repository`.
- Arquitectura frontend: módulos por `pages`, `components`, `hooks`, `modals`, `services`, `types`.

---

## Principios arquitectónicos

### 1. Separación de capas

Cada capa debe cumplir una función clara:

```text
Controller  -> Transporte HTTP
Service     -> Reglas de negocio
Repository  -> Acceso a datos
FormRequest -> Validación
Policy/Gate -> Autorización
Resource    -> Transformación de salida
Job         -> Procesamiento asíncrono
```

Ninguna capa debe absorber responsabilidades de otra. Si una capa empieza a crecer demasiado, debe dividirse.

### 2. Una responsabilidad por archivo

Cada archivo debe tener un motivo claro de existencia. No debe mezclarse:

- Validación con persistencia.
- UI con llamadas HTTP directas.
- Reglas de negocio con queries SQL.
- Autorización con transformación de respuesta.
- Sincronización con renderizado.
- Modales con páginas gigantes.

### 3. Dominio antes que tecnología

La estructura debe reflejar el dominio del negocio. Los nombres deben ser explícitos:

Correcto:

```text
Orders/
Inventory/
Companies/
Invoices/
Users/
Sync/
Reports/
```

Incorrecto:

```text
Utils/
Misc/
Functions/
General/
NewModule/
TestModule/
```

### 4. Código orientado a casos de uso

Los services deben representar acciones reales del negocio:

```text
CreateOrderService
SyncInventoryService
GenerateInvoiceService
ApproveUserAccessService
```

Evitar services ambiguos:

```text
GeneralService
MainService
HelperService
```

---

## Arquitectura backend

### Estructura recomendada

```text
app/
├── Http/
│   ├── Controllers/
│   │   └── Api/
│   ├── Requests/
│   └── Resources/
├── Services/
├── Repositories/
├── Policies/
├── Jobs/
├── DTOs/
├── Enums/
├── Exceptions/
├── Events/
└── Listeners/
```

### Flujo correcto

```text
Route
  -> Middleware auth
  -> FormRequest
  -> Controller
  -> Policy/Gate
  -> Service
  -> Repository
  -> Model/DB
  -> Resource
  -> JSON Response
```

### Controller

Responsabilidades permitidas:

- Recibir request validado.
- Obtener usuario autenticado.
- Obtener `companyId` desde token/contexto autenticado.
- Invocar service.
- Retornar Resource o JSON response.
- Delegar autorización a Policy/Gate.

Prohibido:

- Consultar DB directamente.
- Hacer joins, filtros o queries.
- Contener reglas de negocio.
- Calcular totales complejos.
- Ejecutar procesos batch.
- Manejar lógica de sincronización.

### Service

Responsabilidades permitidas:

- Reglas de negocio.
- Orquestación de repositorios.
- Transacciones.
- Validaciones de dominio.
- Decisiones condicionales del negocio.
- Llamada a jobs, eventos o notificaciones.
- Coordinación de auditoría.

Prohibido:

- Depender directamente de `Request`.
- Retornar vistas.
- Construir JSX o HTML frontend.
- Ejecutar SQL crudo innecesario.
- Ignorar autorización u ownership.

### Repository

Responsabilidades permitidas:

- Queries.
- Filtros.
- Paginación.
- Upserts.
- Búsquedas.
- Agregaciones.
- Bloqueos pesimistas cuando corresponda.
- Operaciones específicas de persistencia.

Prohibido:

- Reglas de negocio.
- Autorización.
- Validación HTTP.
- Manejo de respuesta JSON.
- Envío de emails.
- Eventos de negocio.

---

## Arquitectura frontend

### Estructura recomendada

```text
src/modules/{module-name}/
├── pages/
├── components/
├── hooks/
├── modals/
├── services/
├── types/
├── utils/
└── constants/
```

### Flujo correcto

```text
Page
  -> Component
  -> Hook
  -> Service
  -> API
```

### Page

Responsabilidades:

- Componer layout principal.
- Conectar hooks del módulo.
- Pasar props a componentes.
- Renderizar estados globales de la pantalla.

No debe:

- Contener lógica de negocio extensa.
- Hacer fetch directo.
- Contener modales grandes inline.
- Contener transformaciones pesadas.

### Component

Responsabilidades:

- Renderizar UI.
- Recibir datos por props.
- Emitir eventos por callbacks.
- Mantener estado local simple de UI.

No debe:

- Hacer fetch directo.
- Conocer endpoints.
- Leer tokens.
- Duplicar lógica del hook.
- Ejecutar validaciones de negocio profundas.

### Hook

Responsabilidades:

- Orquestar estado.
- Consumir `useSWR` o `react-query`.
- Encapsular acciones del módulo.
- Manejar loading, error, mutate/refetch.
- Cancelar efectos con cleanup.

No debe:

- Contener JSX.
- Renderizar componentes.
- Mezclar múltiples dominios sin separación.
- Manipular DOM directamente salvo necesidad controlada.

### Modal

Responsabilidades:

- Ser independiente.
- Recibir `open`, `onClose` y datos necesarios.
- Gestionar formulario interno si aplica.
- Delegar acciones al hook o callbacks.

No debe:

- Estar incrustado como bloque enorme dentro de la página.
- Hacer fetch directo si esa data pertenece al módulo.
- Romper accesibilidad de foco.

---

## Límites por archivo

| Tipo de archivo | Límite recomendado |
|---|---:|
| Controller | 80-150 líneas |
| Service | 150-300 líneas |
| Repository | 120-250 líneas |
| FormRequest | 40-120 líneas |
| Policy | 40-150 líneas |
| React Page | 100-220 líneas |
| React Component | 80-180 líneas |
| Hook | 40-140 líneas |
| Modal | 80-180 líneas |
| Service frontend | 40-140 líneas |

Si un archivo excede el límite, dividir por responsabilidad antes de continuar.

---

## Patrones permitidos

- Controller-Service-Repository.
- FormRequest para validación.
- Policy/Gate para autorización.
- DTOs para transporte interno cuando haya estructuras complejas.
- Resources para salida API.
- Jobs para procesos pesados.
- Events/Listeners para efectos secundarios.
- Transactions para operaciones atómicas.
- Repository methods explícitos.
- Hooks por caso de uso.
- Modales independientes.
- React Query o SWR para datos remotos.
- Code splitting por módulo.
- Cache con invalidación controlada.

---

## Patrones prohibidos

- Fat Controller.
- God Service.
- Repository con negocio.
- Helper global con lógica crítica.
- Componentes con fetch directo.
- Hooks con JSX.
- Modales gigantes dentro de pages.
- SQL crudo concatenando strings.
- `company_id` enviado por frontend como fuente de verdad.
- Endpoints sin ownership.
- Sync sin deduplicación.
- Upsert masivo sin chunks.
- Transacciones sin rollback.
- Logs con tokens, contraseñas o datos sensibles.
- Archivos con responsabilidades mezcladas.
- `any` como salida por defecto en TypeScript.
- Silenciar errores sin logging controlado.

---

## Decisiones arquitectónicas obligatorias

Antes de implementar, Antigravity debe responder internamente:

1. ¿Qué capa debe contener esta lógica?
2. ¿Existe ya un patrón similar en el proyecto?
3. ¿El usuario autenticado tiene permiso?
4. ¿El recurso pertenece a la empresa del usuario?
5. ¿La query requiere índice?
6. ¿El flujo necesita transacción?
7. ¿El proceso debe ser un job?
8. ¿Hay riesgo de duplicados?
9. ¿El componente puede dividirse?
10. ¿Hay pruebas necesarias para este cambio?

---

## Entregable arquitectónico mínimo

Toda entrega debe indicar:

- Archivos creados o modificados.
- Responsabilidad de cada archivo.
- Decisiones de separación de capas.
- Riesgos técnicos.
- Pruebas recomendadas.
- Validaciones de seguridad aplicadas.
