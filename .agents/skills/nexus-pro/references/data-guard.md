# DataGuard — Type-Safe Data Hydration

## Purpose

Defines the DataGuard pattern: a universal sanitization layer that guarantees safe data shapes when consuming data from IndexedDB, external APIs, localStorage, or any untrusted source.

The root cause of `items.map is not a function` and similar runtime crashes is trusting that incoming data has the expected shape. DataGuard eliminates this class of bug entirely.

---

## The Core Problem

```typescript
// This crashes when items is null, undefined, a string, or an object
const total = order.items.map(i => i.price).reduce((a, b) => a + b, 0);

// This crashes when the API returns {data: null} instead of {data: []}
const names = response.data.map(u => u.name);
```

These errors are silent, unpredictable, and hard to reproduce. DataGuard makes them impossible.

---

## DataGuard Implementation

```typescript
// shared/utils/data-guard.ts

export class DataGuard {

  // ── Primitive Sanitizers ──────────────────────────────────────

  static string(value: unknown, fallback = ''): string {
    if (typeof value === 'string') return value;
    if (value === null || value === undefined) return fallback;
    return String(value);
  }

  static number(value: unknown, fallback = 0): number {
    const n = Number(value);
    return isNaN(n) ? fallback : n;
  }

  static boolean(value: unknown, fallback = false): boolean {
    if (typeof value === 'boolean') return value;
    if (value === 1 || value === '1' || value === 'true') return true;
    if (value === 0 || value === '0' || value === 'false') return false;
    return fallback;
  }

  static array<T>(value: unknown, fallback: T[] = []): T[] {
    return Array.isArray(value) ? value as T[] : fallback;
  }

  static object<T extends object>(value: unknown, fallback: T): T {
    if (value !== null && typeof value === 'object' && !Array.isArray(value)) {
      return value as T;
    }
    return fallback;
  }

  // ── Date Sanitizers ───────────────────────────────────────────

  static date(value: unknown): Date | null {
    if (!value) return null;
    const d = new Date(value as string);
    return isNaN(d.getTime()) ? null : d;
  }

  static timestamp(value: unknown, fallback = 0): number {
    const n = Number(value);
    return isNaN(n) || n <= 0 ? fallback : n;
  }

  // ── Entity Sanitizers (extend for your domain) ────────────────

  static sanitizeOrder(raw: unknown): SafeOrder {
    const obj = DataGuard.object(raw, {});
    return {
      id:         DataGuard.string((obj as any).id),
      localId:    DataGuard.string((obj as any).localId, crypto.randomUUID()),
      status:     DataGuard.string((obj as any).status, 'PENDING'),
      total:      DataGuard.number((obj as any).total),
      items:      DataGuard.array((obj as any).items).map(DataGuard.sanitizeOrderItem),
      createdAt:  DataGuard.timestamp((obj as any).createdAt, Date.now()),
      synced:     DataGuard.boolean((obj as any).synced),
    };
  }

  static sanitizeOrderItem(raw: unknown): SafeOrderItem {
    const obj = DataGuard.object(raw, {});
    return {
      productId: DataGuard.string((obj as any).productId),
      name:      DataGuard.string((obj as any).name, 'Unknown product'),
      quantity:  DataGuard.number((obj as any).quantity, 1),
      price:     DataGuard.number((obj as any).price),
    };
  }

  // ── API Response Sanitizer ─────────────────────────────────────

  static sanitizeListResponse<T>(
    raw: unknown,
    itemSanitizer: (item: unknown) => T
  ): { data: T[]; total: number } {
    const obj = DataGuard.object(raw, {});
    return {
      data:  DataGuard.array((obj as any).data).map(itemSanitizer),
      total: DataGuard.number((obj as any).total ?? (obj as any).meta?.total),
    };
  }
}

// Types (extend for your domain)
interface SafeOrder {
  id: string;
  localId: string;
  status: string;
  total: number;
  items: SafeOrderItem[];
  createdAt: number;
  synced: boolean;
}

interface SafeOrderItem {
  productId: string;
  name: string;
  quantity: number;
  price: number;
}
```

---

## Usage Patterns

### API responses

```typescript
// ❌ FORBIDDEN — trusting API shape
const { data } = await apiClient.get('/orders');
setOrders(data.data.map(o => o)); // crashes if data.data is null

// ✅ REQUIRED — always sanitize
const response = await apiClient.get('/orders');
const { data, total } = DataGuard.sanitizeListResponse(response.data, DataGuard.sanitizeOrder);
setOrders(data);
setTotal(total);
```

### IndexedDB reads

```typescript
// ❌ FORBIDDEN
const raw = await db.getAll('orders');
const pending = raw.filter(o => o.status === 'PENDING');

// ✅ REQUIRED
const raw = await db.getAll('orders');
const orders = raw.map(DataGuard.sanitizeOrder);
const pending = orders.filter(o => o.status === 'PENDING');
```

### Component props

```typescript
// ❌ FORBIDDEN
function OrderSummary({ order }: { order: Order }) {
  const subtotal = order.items.reduce((a, i) => a + i.price * i.quantity, 0);
  // crashes if items is not an array
}

// ✅ REQUIRED
function OrderSummary({ order }: { order: Order }) {
  const items = Array.isArray(order.items) ? order.items : [];
  const subtotal = items.reduce((a, i) => a + (i.price ?? 0) * (i.quantity ?? 1), 0);
}
```

---

## How to Extend DataGuard for Your Domain

1. Add a `sanitize[YourEntity]` static method to the DataGuard class
2. Use only `DataGuard.*` primitives inside — no direct casting
3. Every field must have a safe fallback
4. Nested arrays or objects must recursively sanitize their children

```typescript
static sanitizeProduct(raw: unknown): SafeProduct {
  const obj = DataGuard.object(raw, {});
  return {
    id:       DataGuard.string((obj as any).id),
    name:     DataGuard.string((obj as any).name, 'Unnamed product'),
    price:    DataGuard.number((obj as any).price),
    stock:    DataGuard.number((obj as any).stock),
    active:   DataGuard.boolean((obj as any).active, true),
    variants: DataGuard.array((obj as any).variants).map(DataGuard.sanitizeVariant),
  };
}
```

---

## Checklist

- [ ] DataGuard class exists in `shared/utils/data-guard.ts`
- [ ] Every IndexedDB read passes through a `DataGuard.sanitize*` method
- [ ] Every API response is sanitized before `setState`
- [ ] No direct `.map()` without `Array.isArray()` check or `DataGuard.array()`
- [ ] No `(data as any).field` without a fallback value
- [ ] Every entity has a corresponding `sanitize[Entity]` method in DataGuard
- [ ] Nested arrays inside entities are recursively sanitized
