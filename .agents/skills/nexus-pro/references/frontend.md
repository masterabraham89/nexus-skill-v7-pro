# Frontend Reference

## Propósito

Este documento define las reglas frontend para React / TypeScript en NEXUS-PRO. El frontend debe ser modular, tipado, accesible, performante, mantenible y seguro.

---

## Estructura modular

Cada dominio debe vivir en su propio módulo:

```text
src/modules/customers/
├── pages/
├── components/
├── hooks/
├── modals/
├── services/
├── types/
├── utils/
└── constants/
```

---

## React best practices

### Reglas base

- Componentes puros siempre que sea posible.
- Hooks para lógica de estado y datos.
- Services para llamadas HTTP.
- Types para contratos.
- Modales independientes.
- Evitar fetch directo en componentes.
- Evitar componentes gigantes.
- Evitar `any`.
- Manejar loading, error, empty y success states.
- Usar `useSWR` o `react-query` para datos remotos.
- Usar Error Boundaries en zonas críticas.
- Aplicar code splitting en módulos pesados.

---

## Componentes puros

Un componente puro debe:

- Recibir datos por props.
- Renderizar UI.
- Emitir eventos mediante callbacks.
- Evitar efectos secundarios innecesarios.
- No conocer endpoints.
- No llamar directamente a APIs.

Ejemplo:

```tsx
type CustomerCardProps = {
  customer: Customer;
  onEdit: (customer: Customer) => void;
};

export function CustomerCard({ customer, onEdit }: CustomerCardProps) {
  return (
    <article>
      <h3>{customer.name}</h3>
      <button onClick={() => onEdit(customer)}>Editar</button>
    </article>
  );
}
```

---

## Hooks

Los hooks encapsulan lógica reutilizable.

Deben:

- Consumir services.
- Manejar estado.
- Usar SWR o react-query.
- Exponer acciones limpias.
- Cancelar efectos cuando aplique.
- No contener JSX.

Prohibido:

```tsx
function useCustomerModal() {
  return <Modal />; // Prohibido
}
```

Correcto:

```tsx
export function useCustomers() {
  const { data, error, isLoading, mutate } = useSWR(
    '/customers',
    customerService.list
  );

  return {
    customers: data ?? [],
    isLoading,
    error,
    refresh: mutate,
  };
}
```

---

## Modales

Los modales deben ser independientes.

Estructura recomendada:

```text
modals/
└── CustomerFormModal.tsx
```

Props mínimas:

```tsx
type CustomerFormModalProps = {
  open: boolean;
  onClose: () => void;
  customer?: Customer;
  onSubmit: (payload: CustomerPayload) => Promise<void>;
};
```

Reglas:

- No incrustar modales largos dentro de pages.
- Deben manejar foco.
- Deben cerrar con Escape si el patrón UI lo permite.
- Deben tener título accesible.
- Deben evitar fetch directo.
- Deben recibir datos y callbacks.

---

## useSWR o react-query

Para datos remotos usar una de estas opciones:

- `useSWR`.
- `@tanstack/react-query`.

### Reglas

- Centralizar keys.
- Manejar cache.
- Invalidar datos después de mutaciones.
- No duplicar requests innecesarios.
- Manejar errores de API.
- Implementar retry con cuidado.
- Evitar retry automático en errores 401/403/422.
- Usar optimistic update solo si el rollback está claro.

---

## AbortController

Usar AbortController cuando se hagan requests manuales o efectos cancelables.

Ejemplo:

```tsx
useEffect(() => {
  const controller = new AbortController();

  loadData({ signal: controller.signal });

  return () => {
    controller.abort();
  };
}, []);
```

Regla obligatoria: todo `useEffect` con listeners, timers, subscriptions, observers o requests manuales debe tener cleanup.

---

## Memoización

Usar memoización cuando exista una razón clara:

- Cálculos pesados.
- Listas grandes.
- Callbacks pasados a componentes memoizados.
- Dependencias estables para hooks.

Herramientas:

- `useMemo`.
- `useCallback`.
- `React.memo`.

No usar memoización indiscriminada. La memoización también tiene costo.

---

## Render performance

Evitar:

- Renderizar listas grandes sin virtualización.
- Crear objetos inline innecesarios en listas.
- Pasar callbacks inestables a muchos hijos.
- Recalcular filtros pesados en cada render.
- Guardar estado duplicado.
- Actualizar estado global para cambios locales.

Aplicar:

- Paginación.
- Virtualización.
- Debounce en búsqueda.
- Lazy loading.
- Code splitting.
- Suspense cuando el proyecto lo soporte.
- Separación de componentes.

---

## Estado global

Usar estado global solo para:

- Usuario autenticado.
- Tema.
- Permisos.
- Configuración global.
- Preferencias persistentes.
- Estado compartido real entre módulos.

No usar estado global para:

- Formularios locales.
- Modales simples.
- Filtros de una sola pantalla.
- Datos remotos que ya maneja SWR/react-query.

---

## Props drilling

Si una prop baja más de 2 o 3 niveles, evaluar:

- Composición de componentes.
- Context específico del módulo.
- Hook compartido.
- Estado local más cercano.
- Reestructuración de componentes.

No crear context global para resolver props drilling pequeño.

---

## Code splitting

Aplicar code splitting en:

- Páginas administrativas pesadas.
- Reportes.
- Dashboards.
- Módulos poco usados.
- Componentes con librerías grandes.
- Editores, gráficos, mapas o tablas avanzadas.

Ejemplo:

```tsx
const ReportsPage = lazy(() => import('./pages/ReportsPage'));
```

---

## UI patterns obligatorios

Toda pantalla debe considerar:

- Estado de carga.
- Estado de error.
- Estado vacío.
- Estado sin permisos.
- Estado de éxito.
- Confirmación para acciones destructivas.
- Feedback visual tras guardar.
- Validación visible.
- Navegación por teclado.
- Mensajes claros.

---

## Seguridad frontend

- No exponer secretos.
- No guardar tokens sensibles en `localStorage` si hay alternativa más segura.
- Sanitizar HTML si se renderiza contenido dinámico.
- Evitar `dangerouslySetInnerHTML`.
- Validar archivos antes de subir.
- Limitar tamaño de uploads.
- No confiar en permisos frontend como única barrera.
- Ocultar acciones no permitidas, pero validar siempre en backend.
- Manejar 401/403 sin revelar información sensible.

---

## Checklist frontend

Antes de entregar:

- [ ] Componentes sin fetch directo.
- [ ] Hooks sin JSX.
- [ ] Modales independientes.
- [ ] Services encapsulan API.
- [ ] Types definidos.
- [ ] Loading/error/empty state cubiertos.
- [ ] Cleanup en useEffect.
- [ ] Sin secretos en frontend.
- [ ] Sin `any` injustificado.
- [ ] Accesibilidad básica revisada.
- [ ] Render performance revisado.
- [ ] Code splitting considerado.
