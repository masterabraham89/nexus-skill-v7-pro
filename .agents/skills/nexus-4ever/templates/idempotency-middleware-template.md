# Idempotency Middleware Template

```php
<?php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;

class EnsureIdempotencyKey
{
    public function handle(Request $request, Closure $next)
    {
        $key = $request->header('Idempotency-Key');

        if (! $key) {
            return response()->json([
                'success' => false,
                'code' => 'IDEMPOTENCY_KEY_REQUIRED',
                'message' => 'La operación requiere Idempotency-Key.',
                'errors' => [],
            ], 400);
        }

        // 1. Calcular payload hash.
        // 2. Buscar key por company_id, user_id, route.
        // 3. Si existe y hash coincide, devolver respuesta guardada.
        // 4. Si existe y hash no coincide, devolver 409.
        // 5. Si no existe, bloquear y continuar.

        return $next($request);
    }
}
```

## Checklist

- [ ] Key requerida.
- [ ] Payload hash.
- [ ] Lock.
- [ ] Respuesta persistida.
- [ ] Conflicto 409.
- [ ] Expiración.
