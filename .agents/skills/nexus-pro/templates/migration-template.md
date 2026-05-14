# Safe Migration Template

## Checklist previo

- [ ] ¿Es destructiva?
- [ ] ¿Requiere expand/contract?
- [ ] ¿La tabla es grande?
- [ ] ¿Puede bloquear producción?
- [ ] ¿Hay rollback?
- [ ] ¿Hay backup?

## Laravel migration base

```php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up(): void
    {
        Schema::table('table_name', function (Blueprint $table) {
            $table->string('new_column')->nullable();
        });
    }

    public function down(): void
    {
        Schema::table('table_name', function (Blueprint $table) {
            $table->dropColumn('new_column');
        });
    }
};
```

## Backfill por lotes

Crear command/job separado si hay muchos datos.

- chunks de 200.
- idempotente.
- logs de progreso.
- reejecutable.
