# File Upload Service Template

```php
<?php

namespace App\Services\Files;

use App\Models\User;
use Illuminate\Http\UploadedFile;
use Illuminate\Support\Facades\Storage;
use Illuminate\Support\Str;

class SecureFileUploadService
{
    public function upload(User $user, UploadedFile $file, string $purpose): string
    {
        $companyId = (int) $user->company_id;

        $this->validateRealMime($file);

        $extension = strtolower($file->getClientOriginalExtension());
        $filename = (string) Str::uuid() . '.' . $extension;
        $path = "company/{$companyId}/{$purpose}/{$filename}";

        Storage::disk('private')->putFileAs(
            dirname($path),
            $file,
            basename($path)
        );

        return $path;
    }

    private function validateRealMime(UploadedFile $file): void
    {
        // Validar MIME real y allowlist según caso de uso.
    }
}
```

## Checklist

- [ ] Tamaño máximo.
- [ ] MIME real.
- [ ] Extensión allowlist.
- [ ] Nombre aleatorio.
- [ ] Storage privado.
- [ ] Sin path controlado por cliente.
- [ ] Auditoría si aplica.
