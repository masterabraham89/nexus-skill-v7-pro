# Frontend Module Template

## Ruta sugerida

```text
src/modules/{module-name}/
├── pages/
│   └── ModulePage.tsx
├── components/
│   ├── ModuleList.tsx
│   └── ModuleCard.tsx
├── hooks/
│   └── useModuleItems.ts
├── modals/
│   └── ModuleFormModal.tsx
├── services/
│   └── moduleService.ts
├── types/
│   └── module.types.ts
└── utils/
    └── module.utils.ts
```

---

## types/module.types.ts

```ts
export type ModuleItem = {
  id: number;
  name: string;
  status: 'active' | 'inactive';
  createdAt: string;
};

export type ModuleItemPayload = {
  name: string;
  status: 'active' | 'inactive';
};

export type ModuleItemListResponse = {
  data: ModuleItem[];
  meta?: {
    currentPage: number;
    lastPage: number;
    total: number;
  };
};
```

---

## services/moduleService.ts

```ts
import type {
  ModuleItem,
  ModuleItemPayload,
  ModuleItemListResponse,
} from '../types/module.types';

const BASE_URL = '/api/module-items';

async function request<T>(url: string, options?: RequestInit): Promise<T> {
  const response = await fetch(url, {
    ...options,
    headers: {
      'Content-Type': 'application/json',
      Accept: 'application/json',
      ...options?.headers,
    },
  });

  if (!response.ok) {
    throw new Error('No se pudo completar la solicitud.');
  }

  return response.json() as Promise<T>;
}

export const moduleService = {
  list(): Promise<ModuleItemListResponse> {
    return request<ModuleItemListResponse>(BASE_URL);
  },

  create(payload: ModuleItemPayload): Promise<ModuleItem> {
    return request<ModuleItem>(BASE_URL, {
      method: 'POST',
      body: JSON.stringify(payload),
    });
  },

  update(id: number, payload: ModuleItemPayload): Promise<ModuleItem> {
    return request<ModuleItem>(`${BASE_URL}/${id}`, {
      method: 'PUT',
      body: JSON.stringify(payload),
    });
  },

  remove(id: number): Promise<void> {
    return request<void>(`${BASE_URL}/${id}`, {
      method: 'DELETE',
    });
  },
};
```

---

## hooks/useModuleItems.ts

```ts
import useSWR from 'swr';
import { moduleService } from '../services/moduleService';
import type { ModuleItemPayload } from '../types/module.types';

const MODULE_ITEMS_KEY = '/module-items';

export function useModuleItems() {
  const { data, error, isLoading, mutate } = useSWR(
    MODULE_ITEMS_KEY,
    moduleService.list
  );

  async function createItem(payload: ModuleItemPayload) {
    await moduleService.create(payload);
    await mutate();
  }

  async function updateItem(id: number, payload: ModuleItemPayload) {
    await moduleService.update(id, payload);
    await mutate();
  }

  async function removeItem(id: number) {
    await moduleService.remove(id);
    await mutate();
  }

  return {
    items: data?.data ?? [],
    meta: data?.meta,
    isLoading,
    error,
    createItem,
    updateItem,
    removeItem,
    refresh: mutate,
  };
}
```

---

## components/ModuleList.tsx

```tsx
import type { ModuleItem } from '../types/module.types';

type ModuleListProps = {
  items: ModuleItem[];
  onEdit: (item: ModuleItem) => void;
  onDelete: (item: ModuleItem) => void;
};

export function ModuleList({ items, onEdit, onDelete }: ModuleListProps) {
  if (items.length === 0) {
    return <p>No hay registros disponibles.</p>;
  }

  return (
    <div>
      {items.map((item) => (
        <article key={item.id}>
          <h3>{item.name}</h3>
          <p>{item.status}</p>

          <button type="button" onClick={() => onEdit(item)}>
            Editar
          </button>

          <button type="button" onClick={() => onDelete(item)}>
            Eliminar
          </button>
        </article>
      ))}
    </div>
  );
}
```

---

## modals/ModuleFormModal.tsx

```tsx
import { useEffect, useState } from 'react';
import type { ModuleItem, ModuleItemPayload } from '../types/module.types';

type ModuleFormModalProps = {
  open: boolean;
  item?: ModuleItem | null;
  onClose: () => void;
  onSubmit: (payload: ModuleItemPayload) => Promise<void>;
};

export function ModuleFormModal({
  open,
  item,
  onClose,
  onSubmit,
}: ModuleFormModalProps) {
  const [name, setName] = useState('');

  useEffect(() => {
    if (open) {
      setName(item?.name ?? '');
    }

    return () => {
      setName('');
    };
  }, [open, item]);

  if (!open) {
    return null;
  }

  async function handleSubmit(event: React.FormEvent<HTMLFormElement>) {
    event.preventDefault();

    await onSubmit({
      name,
      status: item?.status ?? 'active',
    });

    onClose();
  }

  return (
    <div role="dialog" aria-modal="true" aria-labelledby="module-modal-title">
      <h2 id="module-modal-title">
        {item ? 'Editar registro' : 'Crear registro'}
      </h2>

      <form onSubmit={handleSubmit}>
        <label htmlFor="module-name">Nombre</label>
        <input
          id="module-name"
          value={name}
          onChange={(event) => setName(event.target.value)}
          maxLength={120}
          required
        />

        <button type="submit">Guardar</button>
        <button type="button" onClick={onClose}>
          Cancelar
        </button>
      </form>
    </div>
  );
}
```

---

## pages/ModulePage.tsx

```tsx
import { useState } from 'react';
import { ModuleList } from '../components/ModuleList';
import { ModuleFormModal } from '../modals/ModuleFormModal';
import { useModuleItems } from '../hooks/useModuleItems';
import type { ModuleItem, ModuleItemPayload } from '../types/module.types';

export function ModulePage() {
  const {
    items,
    isLoading,
    error,
    createItem,
    updateItem,
    removeItem,
  } = useModuleItems();

  const [selectedItem, setSelectedItem] = useState<ModuleItem | null>(null);
  const [isModalOpen, setIsModalOpen] = useState(false);

  function handleCreate() {
    setSelectedItem(null);
    setIsModalOpen(true);
  }

  function handleEdit(item: ModuleItem) {
    setSelectedItem(item);
    setIsModalOpen(true);
  }

  async function handleSubmit(payload: ModuleItemPayload) {
    if (selectedItem) {
      await updateItem(selectedItem.id, payload);
      return;
    }

    await createItem(payload);
  }

  if (isLoading) {
    return <p>Cargando registros...</p>;
  }

  if (error) {
    return <p>No se pudieron cargar los registros.</p>;
  }

  return (
    <section>
      <header>
        <h1>Módulo</h1>
        <button type="button" onClick={handleCreate}>
          Crear
        </button>
      </header>

      <ModuleList
        items={items}
        onEdit={handleEdit}
        onDelete={removeItem}
      />

      <ModuleFormModal
        open={isModalOpen}
        item={selectedItem}
        onClose={() => setIsModalOpen(false)}
        onSubmit={handleSubmit}
      />
    </section>
  );
}
```

---

## Checklist frontend module

- [ ] Page compone, no concentra toda la lógica.
- [ ] Components no hacen fetch directo.
- [ ] Hook no contiene JSX.
- [ ] Modal independiente.
- [ ] Service encapsula API.
- [ ] Types definidos.
- [ ] Loading state.
- [ ] Error state.
- [ ] Empty state.
- [ ] Cleanup en `useEffect`.
- [ ] Accesibilidad básica.
- [ ] Sin secretos en frontend.
