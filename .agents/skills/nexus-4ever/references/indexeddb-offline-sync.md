# IndexedDB + Offline Sync Patterns

## Purpose

Defines patterns for safe offline-first data management using IndexedDB, including type-safe hydration, deduplication, stale data protection, and reliable sync to the cloud backend.

Use when `features.offline_sync: true` in `NEXUS_CONFIG.md` or when the project requires offline operation.

---

## Core Rules

### 1. Never Trust Raw IndexedDB Data

Data from IndexedDB must always be validated before use. IndexedDB stores raw objects — corrupted records, partial writes, or schema changes can produce unexpected shapes.

```typescript
// ❌ FORBIDDEN — direct use without validation
const items = await db.getAll('orders');
items.map(item => item.products.length); // crash if products is not an array

// ✅ REQUIRED — always sanitize through DataGuard
import { DataGuard } from '@/shared/utils/data-guard';

const raw = await db.getAll('orders');
const items = raw.map(DataGuard.sanitizeOrder); // guaranteed safe shape
```

### 2. Arrays Must Be Validated Before .map()

```typescript
// ❌ FORBIDDEN
const total = order.items.map(i => i.price).reduce(...);

// ✅ REQUIRED
const items = Array.isArray(order.items) ? order.items : [];
const total = items.map(i => i.price ?? 0).reduce((a, b) => a + b, 0);
```

### 3. Sync Must Be Idempotent

Running a sync operation twice must not create duplicate records. Use stable unique keys.

```typescript
// ✅ Upsert with external_id as idempotency key
await apiClient.post('/sync/orders', {
  items: pendingOrders.map(order => ({
    ...order,
    idempotency_key: order.localId, // stable key from IndexedDB
  }))
});
```

### 4. SyncOrchestrator Must Have AbortController

```typescript
export class SyncOrchestrator {
  private controller: AbortController | null = null;
  private isRunning = false; // prevents duplicate concurrent syncs

  async start(): Promise<void> {
    if (this.isRunning) {
      console.warn('SyncOrchestrator: sync already in progress, skipping');
      return;
    }

    this.isRunning = true;
    this.controller = new AbortController();

    try {
      await this.syncPendingOrders(this.controller.signal);
    } finally {
      this.isRunning = false;
      this.controller = null;
    }
  }

  stop(): void {
    this.controller?.abort();
    this.isRunning = false;
  }
}
```

### 5. Stale Data Protection — Timestamp-Based Conflict Resolution

Newer data always wins. Never overwrite a newer record with an older one.

```typescript
interface SyncRecord {
  id: string;
  updatedAt: number; // Unix timestamp in ms
  version: number;   // increment on every local change
}

function shouldOverwrite(existing: SyncRecord, incoming: SyncRecord): boolean {
  // Incoming wins only if it's strictly newer
  return incoming.updatedAt > existing.updatedAt ||
    (incoming.updatedAt === existing.updatedAt && incoming.version > existing.version);
}

async function mergeRecord(incoming: SyncRecord): Promise<void> {
  const existing = await db.get('records', incoming.id);

  if (!existing || shouldOverwrite(existing, incoming)) {
    await db.put('records', incoming);
  }
  // else: keep existing — it's newer
}
```

### 6. IndexedDB Schema Versioning

Always handle schema upgrades gracefully.

```typescript
const db = await openDB('app-db', 3, {
  upgrade(db, oldVersion, newVersion) {
    // Always use switch-fallthrough — handles upgrades from any version
    switch (oldVersion) {
      case 0:
        db.createObjectStore('orders', { keyPath: 'localId' });
        // fallthrough
      case 1:
        const store = db.transaction.objectStore('orders');
        store.createIndex('by-status', 'status');
        // fallthrough
      case 2:
        db.createObjectStore('sync_log', { keyPath: 'id', autoIncrement: true });
    }
  }
});
```

### 7. Pending Queue Pattern

Local changes must be queued before sync, not written directly.

```typescript
interface PendingOperation {
  id: string;
  type: 'CREATE' | 'UPDATE' | 'DELETE';
  entity: string;
  payload: unknown;
  createdAt: number;
  retries: number;
  lastError?: string;
}

// Queue first — sync later
async function queueOperation(op: Omit<PendingOperation, 'id' | 'createdAt' | 'retries'>): Promise<void> {
  await db.add('pending_queue', {
    ...op,
    id: crypto.randomUUID(),
    createdAt: Date.now(),
    retries: 0,
  });
}
```

---

## Prohibited Patterns

```typescript
// ❌ Never assume IndexedDB data shape
const order = await db.get('orders', id);
order.items.forEach(...); // order or items could be undefined

// ❌ Never sync without abort support
setInterval(() => syncAll(), 5000); // no way to cancel

// ❌ Never use Date.now() as unique ID
const id = Date.now().toString(); // collisions on fast operations

// ❌ Never skip error handling on IndexedDB operations
await db.put('orders', record); // what if storage quota exceeded?
```

---

## Checklist

- [ ] All IndexedDB reads pass through DataGuard sanitization
- [ ] All array fields validated with `Array.isArray()` before `.map()`
- [ ] SyncOrchestrator has `isRunning` guard to prevent duplicate syncs
- [ ] SyncOrchestrator has AbortController for cleanup on unmount
- [ ] Sync operations use idempotency keys (not auto-increment IDs)
- [ ] Conflict resolution uses timestamp + version comparison
- [ ] IndexedDB schema has versioned `upgrade()` handler
- [ ] Pending operations are queued before syncing, not written directly
- [ ] Storage quota errors are caught and handled gracefully
