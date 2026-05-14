# DDD-lite Use Case Template

## Ruta sugerida

```text
app/Domains/{Domain}/Actions/{ActionName}.php
```

## Plantilla

```php
<?php

namespace App\Domains\Domain\Actions;

use App\Models\User;
use Illuminate\Support\Facades\DB;

final class ActionName
{
    public function __construct(
        private readonly DomainRepository $repository,
    ) {}

    public function execute(User $user, ActionData $data): ActionResult
    {
        $companyId = $this->resolveCompanyId($user);

        return DB::transaction(function () use ($user, $companyId, $data) {
            $this->ensureInvariants($user, $companyId, $data);

            $result = $this->repository->perform($companyId, $data);

            // Domain event, audit or outbox if needed.

            return $result;
        });
    }

    private function resolveCompanyId(User $user): int
    {
        return (int) $user->company_id;
    }

    private function ensureInvariants(User $user, int $companyId, ActionData $data): void
    {
        // Domain rules.
    }
}
```

## Checklist

- [ ] Use case representa acción real.
- [ ] Invariantes protegidas.
- [ ] Transaction si aplica.
- [ ] Events/outbox si aplica.
- [ ] No depende de Request.
