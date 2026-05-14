# Export Job Template

## Objetivo

Plantilla para exports seguros, auditados y privados.

```php
<?php

namespace App\Jobs\Exports;

use App\Services\Exports\ExportAuditService;
use Illuminate\Bus\Queueable;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Foundation\Bus\Dispatchable;
use Illuminate\Queue\InteractsWithQueue;
use Illuminate\Queue\SerializesModels;
use Illuminate\Support\Facades\Storage;

class GenerateDomainExportJob implements ShouldQueue
{
    use Dispatchable, InteractsWithQueue, Queueable, SerializesModels;

    public int $timeout = 300;

    public function __construct(
        public readonly int $companyId,
        public readonly int $actorId,
        public readonly array $filters,
        public readonly string $requestId,
    ) {}

    public function handle(ExportAuditService $audit): void
    {
        // 1. Validar scope por companyId.
        // 2. Aplicar allowlist de columnas.
        // 3. Procesar en chunks.
        // 4. Sanitizar CSV Injection.
        // 5. Guardar en storage privado.
        // 6. Auditar resultado.
    }
}
```

## Checklist

- [ ] Permiso validado antes de dispatch.
- [ ] companyId seguro.
- [ ] Filtros limitados.
- [ ] CSV Injection mitigado.
- [ ] Storage privado.
- [ ] URL temporal.
- [ ] Auditoría.
