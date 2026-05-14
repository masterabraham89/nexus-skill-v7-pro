# Backend Service Template

## Ruta sugerida

```text
app/Services/{Domain}/{ActionName}Service.php
```

## Objetivo

Plantilla base para services Laravel dentro de NEXUS-4EVER.

---

## Estructura recomendada

```php
<?php

namespace App\Services\Domain;

use App\Models\User;
use App\Repositories\Domain\DomainRepository;
use Illuminate\Support\Facades\DB;

class ActionNameService
{
    public function __construct(
        private readonly DomainRepository $repository,
    ) {}

    public function execute(User $user, array $data): mixed
    {
        $companyId = $this->resolveCompanyId($user);

        return DB::transaction(function () use ($user, $companyId, $data) {
            // 1. Validar reglas de negocio.
            $this->ensureBusinessRules($user, $companyId, $data);

            // 2. Ejecutar operación principal.
            $result = $this->repository->createForCompany(
                companyId: $companyId,
                data: $data
            );

            // 3. Registrar auditoría si aplica.
            // $this->audit(...);

            // 4. Despachar eventos/jobs después de confirmar estrategia.
            // Event::dispatch(...);

            return $result;
        });
    }

    private function resolveCompanyId(User $user): int
    {
        // Regla obligatoria:
        // companyId debe venir desde token autenticado o contexto server-side.
        // Nunca confiar en company_id enviado por frontend.

        return (int) $user->company_id;
    }

    private function ensureBusinessRules(User $user, int $companyId, array $data): void
    {
        // Validar ownership, permisos complementarios o reglas de dominio.
        // Las autorizaciones principales deben vivir en Policies/Gates.
    }
}
```

---

## Repository asociado

```php
<?php

namespace App\Repositories\Domain;

use App\Models\DomainModel;

class DomainRepository
{
    public function createForCompany(int $companyId, array $data): DomainModel
    {
        return DomainModel::query()->create([
            ...$data,
            'company_id' => $companyId,
        ]);
    }

    public function findByIdForCompany(int $id, int $companyId): ?DomainModel
    {
        return DomainModel::query()
            ->where('id', $id)
            ->where('company_id', $companyId)
            ->first();
    }
}
```

---

## FormRequest asociado

```php
<?php

namespace App\Http\Requests\Domain;

use Illuminate\Foundation\Http\FormRequest;

class StoreDomainRequest extends FormRequest
{
    public function authorize(): bool
    {
        return true; // Usar Policy/Gate en controller o service según flujo.
    }

    public function rules(): array
    {
        return [
            'name' => ['required', 'string', 'max:120'],
        ];
    }
}
```

---

## Controller asociado

```php
<?php

namespace App\Http\Controllers\Api\Domain;

use App\Http\Controllers\Controller;
use App\Http\Requests\Domain\StoreDomainRequest;
use App\Http\Resources\DomainResource;
use App\Models\DomainModel;
use App\Services\Domain\ActionNameService;

class DomainController extends Controller
{
    public function store(StoreDomainRequest $request, ActionNameService $service): DomainResource
    {
        $this->authorize('create', DomainModel::class);

        $result = $service->execute(
            user: $request->user(),
            data: $request->validated()
        );

        return new DomainResource($result);
    }
}
```

---

## Checklist del service

- [ ] No depende directamente de Request.
- [ ] Recibe usuario autenticado.
- [ ] Resuelve companyId desde contexto seguro.
- [ ] No confía en company_id del cliente.
- [ ] Usa transaction si hay escrituras múltiples.
- [ ] Lanza excepciones ante errores de negocio.
- [ ] No contiene queries complejas directas.
- [ ] Usa repository.
- [ ] Registra auditoría si aplica.
- [ ] No registra secretos.
- [ ] Es testeable.
