# Before / After — Frontend (React + TypeScript)

> These examples show the difference between code generated **without NEXUS-PRO** and code generated **with NEXUS-PRO** active.

---

## Example 1: Customer List Component

### ❌ WITHOUT NEXUS-PRO

```tsx
// CustomerList.tsx — everything in one giant file
export function CustomerList() {
  const [customers, setCustomers] = useState<any[]>([]); // ⚠️ any — no types
  const [loading, setLoading]     = useState(false);

  useEffect(() => {
    setLoading(true);

    // Direct fetch inside component — breaks separation of concerns
    fetch('/api/customers', {
      headers: { Authorization: `Bearer ${localStorage.getItem('token')}` }
      // ⚠️ token printed in component — security anti-pattern
    })
      .then(r => r.json())
      .then(data => {
        setCustomers(data); // ⚠️ no validation — assumes array
        setLoading(false);
      });
    // ⚠️ No error state — silent failure if API is down
    // ⚠️ No cleanup — memory leak on unmount
    // ⚠️ No empty state — blank UI with no message
  }, []);

  // Modal hardcoded inside — can't be reused
  const [showModal, setShowModal] = useState(false);
  const [selected, setSelected]  = useState<any>(null);

  return (
    <div>
      {loading && <p>Loading...</p>}
      {customers.map(c => (
        <div key={c.id} onClick={() => { setSelected(c); setShowModal(true); }}>
          {c.name}
        </div>
      ))}

      {/* 80-line modal hardcoded here */}
      {showModal && (
        <div className="modal">
          {/* ... */}
        </div>
      )}
    </div>
  );
}
```

**Problems:**
- `any` everywhere — TypeScript is useless, errors are invisible
- `fetch` inside component — impossible to reuse or test
- Token read from `localStorage` inside component — security smell
- No error state — API failure = blank screen, user has no idea what happened
- No cleanup — if user navigates away, the request still runs and sets state on unmounted component
- Modal inside page — 200+ line file, impossible to maintain

---

### ✅ WITH NEXUS-PRO

**4 separate files with single responsibility:**

```ts
// modules/customers/types/customer.types.ts
export interface Customer {
  id: number;
  name: string;
  email: string;
  status: 'active' | 'inactive';
  createdAt: string;
}

export interface CustomerListResponse {
  data: Customer[];
  meta: { total: number; page: number; lastPage: number };
}
```

```ts
// modules/customers/services/customer.service.ts
import { apiClient } from '@/shared/services/api-client';
import type { CustomerListResponse } from '../types/customer.types';

export const customerService = {
  list: async (signal?: AbortSignal): Promise<CustomerListResponse> => {
    const response = await apiClient.get('/customers', { signal });
    return response.data;
  },
};
// Auth headers handled centrally in apiClient — never repeated in components
```

```ts
// modules/customers/hooks/useCustomers.ts
import useSWR from 'swr';
import { customerService } from '../services/customer.service';
import type { Customer } from '../types/customer.types';

export function useCustomers() {
  const { data, error, isLoading, mutate } = useSWR(
    'customers',
    () => customerService.list()
  );

  return {
    customers: data?.data ?? [],   // ✅ safe default — never undefined
    total:     data?.meta.total ?? 0,
    isLoading,
    error,
    refresh: mutate,
  };
}
// No JSX — hook is pure logic, fully testable
```

```tsx
// modules/customers/components/CustomerList.tsx
import { useCustomers } from '../hooks/useCustomers';
import { CustomerCard } from './CustomerCard';
import { CustomerFormModal } from '../modals/CustomerFormModal';
import type { Customer } from '../types/customer.types';

export function CustomerList() {
  const { customers, isLoading, error } = useCustomers();
  const [selected, setSelected] = useState<Customer | null>(null);

  // ✅ Every state covered
  if (isLoading) return <LoadingSpinner />;
  if (error)     return <ErrorMessage message="Could not load customers. Please retry." />;
  if (customers.length === 0) return <EmptyState message="No customers yet." />;

  return (
    <>
      <ul>
        {customers.map(customer => (
          <CustomerCard
            key={customer.id}
            customer={customer}
            onEdit={setSelected}   // ✅ pure callback — component doesn't know modal exists
          />
        ))}
      </ul>

      {/* ✅ Modal is independent — reusable anywhere */}
      <CustomerFormModal
        open={selected !== null}
        customer={selected}
        onClose={() => setSelected(null)}
        onSubmit={async (payload) => { /* ... */ }}
      />
    </>
  );
}
```

**What NEXUS-PRO added:**
- ✅ Full TypeScript types — no `any`, errors caught at compile time
- ✅ `fetch` encapsulated in service — one place to change auth headers
- ✅ Hook handles data, loading, error — component is pure UI
- ✅ All 3 UI states covered: loading, error, empty
- ✅ Modal is independent and reusable
- ✅ SWR handles caching, revalidation, and deduplication automatically

---

## Example 2: useEffect with Cleanup

### ❌ WITHOUT NEXUS-PRO

```tsx
useEffect(() => {
  // Starts a polling interval
  const interval = setInterval(() => {
    fetchOrders(); // runs forever even after component unmounts
  }, 5000);

  // Subscribes to a websocket channel
  channel.subscribe('orders', handleOrder); // never unsubscribed

  // Starts a manual fetch
  fetch('/api/orders').then(setOrders); // no AbortController
}, []);
// ⚠️ No cleanup function — 3 memory leaks in one useEffect
```

---

### ✅ WITH NEXUS-PRO

```tsx
useEffect(() => {
  const controller = new AbortController();

  // Manual fetch with cancellation support
  orderService.list({ signal: controller.signal })
    .then(setOrders)
    .catch(err => {
      if (err.name !== 'AbortError') setError(err); // ignore intentional cancels
    });

  // Polling with cleanup
  const interval = setInterval(() => {
    orderService.list({ signal: controller.signal }).then(setOrders);
  }, 5000);

  // WebSocket subscription with cleanup
  const unsubscribe = channel.subscribe('orders', handleOrder);

  // ✅ Cleanup runs on unmount or dependency change
  return () => {
    controller.abort();    // cancel in-flight requests
    clearInterval(interval); // stop polling
    unsubscribe();           // remove listener
  };
}, []);
```

---

## Summary

| Metric | Without NEXUS-PRO | With NEXUS-PRO |
|--------|------------------|----------------|
| Type safety | ❌ `any` everywhere | ✅ Full TypeScript |
| File structure | ❌ 1 giant component | ✅ 4 focused files |
| Fetch location | ❌ Inside component | ✅ In service layer |
| Loading state | ⚠️ Sometimes | ✅ Always |
| Error state | ❌ Missing | ✅ Always |
| Empty state | ❌ Missing | ✅ Always |
| Memory leaks | ❌ Common | ✅ Cleanup enforced |
| Modal reusability | ❌ Hardcoded | ✅ Independent component |
| Testability | ❌ Hard (fetch in component) | ✅ Each layer tested separately |
