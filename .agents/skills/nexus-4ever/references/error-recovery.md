# Error Recovery + Resilience Patterns

## Purpose

Patterns for graceful degradation, retry with exponential backoff, and circuit breaker in frontend applications. Prevents cascading failures when the backend is slow or unavailable.

---

## Core Rules

### 1. Retry with Exponential Backoff

Never retry immediately on failure — exponential backoff prevents hammering a struggling backend.

```typescript
async function withRetry<T>(
  fn: () => Promise<T>,
  options = { maxRetries: 3, baseDelayMs: 1000 }
): Promise<T> {
  let lastError: Error;

  for (let attempt = 0; attempt <= options.maxRetries; attempt++) {
    try {
      return await fn();
    } catch (error) {
      lastError = error as Error;
      if (attempt === options.maxRetries) break;

      // Exponential backoff: 1s, 2s, 4s
      const delay = options.baseDelayMs * Math.pow(2, attempt);
      await new Promise(resolve => setTimeout(resolve, delay));
    }
  }

  throw lastError!;
}

// Usage
const orders = await withRetry(() => orderService.list());
```

### 2. Circuit Breaker

Stop making requests when the backend is clearly unavailable. Reset after a cooldown.

```typescript
class CircuitBreaker {
  private failures = 0;
  private lastFailureTime: number | null = null;
  private readonly threshold = 5;      // open after 5 failures
  private readonly cooldownMs = 60_000; // try again after 60s

  isOpen(): boolean {
    if (this.failures < this.threshold) return false;

    const elapsed = Date.now() - (this.lastFailureTime ?? 0);
    if (elapsed > this.cooldownMs) {
      this.reset(); // cooldown expired — allow one attempt
      return false;
    }
    return true; // still open — block requests
  }

  recordFailure(): void {
    this.failures++;
    this.lastFailureTime = Date.now();
  }

  recordSuccess(): void { this.reset(); }
  private reset(): void { this.failures = 0; this.lastFailureTime = null; }
}

// Usage
const breaker = new CircuitBreaker();

async function safeFetch<T>(fn: () => Promise<T>): Promise<T> {
  if (breaker.isOpen()) {
    throw new Error('Service unavailable — circuit open');
  }
  try {
    const result = await fn();
    breaker.recordSuccess();
    return result;
  } catch (error) {
    breaker.recordFailure();
    throw error;
  }
}
```

### 3. Graceful Degradation

When the backend is down, serve stale cached data instead of showing an error.

```typescript
async function getOrdersWithFallback(): Promise<Order[]> {
  try {
    const fresh = await orderService.list();
    // Cache the fresh response
    localStorage.setItem('orders_cache', JSON.stringify({
      data: fresh,
      cachedAt: Date.now(),
    }));
    return fresh;
  } catch {
    // Backend unavailable — serve stale data
    const cached = localStorage.getItem('orders_cache');
    if (cached) {
      const { data, cachedAt } = JSON.parse(cached);
      const ageMinutes = (Date.now() - cachedAt) / 60_000;
      console.warn(`Serving stale orders data (${ageMinutes.toFixed(0)}m old)`);
      return data;
    }
    return []; // last resort — empty array, not a crash
  }
}
```

### 4. User-Facing Error Classification

Not all errors are equal. Show the right message for each type.

```typescript
function classifyError(error: unknown): {
  type: 'network' | 'auth' | 'server' | 'validation' | 'unknown';
  userMessage: string;
  retryable: boolean;
} {
  if (!navigator.onLine) {
    return { type: 'network', userMessage: 'No internet connection. Changes saved locally.', retryable: true };
  }
  const status = (error as any)?.response?.status;
  if (status === 401 || status === 403) {
    return { type: 'auth', userMessage: 'Session expired. Please log in again.', retryable: false };
  }
  if (status === 422) {
    return { type: 'validation', userMessage: 'Please check the form fields.', retryable: false };
  }
  if (status >= 500) {
    return { type: 'server', userMessage: 'Server error. Please try again in a moment.', retryable: true };
  }
  return { type: 'unknown', userMessage: 'Something went wrong. Please retry.', retryable: true };
}
```

---

## Checklist

- [ ] Critical operations use `withRetry` with max 3 attempts
- [ ] Exponential backoff applied — no immediate retries
- [ ] Circuit breaker prevents request storms on backend failure
- [ ] Stale cache served when backend unavailable (graceful degradation)
- [ ] Error messages classified by type — no raw API errors shown to users
- [ ] Network status (`navigator.onLine`) checked before requests in offline-first apps
