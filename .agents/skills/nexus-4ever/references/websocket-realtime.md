# WebSocket + Real-Time Patterns

## Purpose

Patterns for safe WebSocket/Pusher integration: channel naming, subscription lifecycle, multi-tenant isolation, heartbeat fallbacks, and cleanup on unmount.

Use when `realtime.enabled: true` in `NEXUS_CONFIG.md`.

---

## Core Rules

### 1. Always Clean Up Subscriptions

```typescript
useEffect(() => {
  const channel = pusher.subscribe(`orders-${tenantSlug}`);
  channel.bind('order.created', handleNewOrder);

  // ✅ REQUIRED: cleanup on unmount
  return () => {
    channel.unbind('order.created', handleNewOrder);
    pusher.unsubscribe(`orders-${tenantSlug}`);
  };
}, [tenantSlug]);
```

### 2. Multi-Tenant Channel Naming

```typescript
// ✅ REQUIRED — always tenant-scoped
const channelName = `orders-${tenantSlug}`;   // "orders-acme-corp"

// ❌ FORBIDDEN — global channel
pusher.subscribe('orders'); // all tenants leak into each other
```

### 3. Heartbeat + Polling Fallback

WebSocket connections drop silently. Always implement a polling fallback for critical data.

```typescript
channel.bind('pusher:subscription_succeeded', () => clearInterval(pollInterval));
channel.bind('pusher:subscription_error', () => {
  pollInterval = setInterval(() => fetchLatest(), 30_000);
});
```

### 4. Validate All Incoming Payloads

```typescript
// ✅ REQUIRED — sanitize through DataGuard
channel.bind('order.created', (raw: unknown) => {
  const order = DataGuard.sanitizeOrder(raw);
  if (!order.id) return; // reject malformed payload
  handleNewOrder(order);
});
```

### 5. Backend — Broadcast to Tenant Channel Only

```php
class OrderCreated implements ShouldBroadcast
{
    public function broadcastOn(): array
    {
        return [new PrivateChannel("orders-{$this->order->company->slug}")];
    }

    public function broadcastWith(): array
    {
        return ['id' => $this->order->id, 'status' => $this->order->status];
        // Never broadcast: card numbers, tokens, or full financial records
    }
}
```

---

## Checklist

- [ ] Every `subscribe` has `unsubscribe` in cleanup
- [ ] Every `bind` has `unbind` in cleanup
- [ ] Channel names include tenant identifier
- [ ] Polling fallback exists for critical features
- [ ] All incoming payloads sanitized through DataGuard
- [ ] Private channels used for sensitive data
