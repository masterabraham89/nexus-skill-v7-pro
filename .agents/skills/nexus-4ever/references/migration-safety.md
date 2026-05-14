# Migration Safety Reference

## Propósito

Este documento define reglas para migraciones Laravel/PostgreSQL seguras, especialmente en producción.

---

## Principios

- Las migraciones deben ser reversibles cuando sea razonable.
- No modificar migraciones antiguas ya ejecutadas.
- Evitar locks largos.
- Evitar cambios destructivos directos.
- Separar expansión y contracción.
- Probar en staging con datos representativos.

---

## Expand / Contract Pattern

### Expand

1. Agregar columna nullable.
2. Agregar tabla nueva.
3. Agregar índice sin romper lecturas.
4. Escribir código compatible con esquema viejo y nuevo.

### Migrate

1. Backfill por lotes.
2. Validar consistencia.
3. Activar nuevo camino con feature flag.

### Contract

1. Eliminar uso del campo viejo.
2. Confirmar que no hay lecturas/escrituras.
3. Eliminar columna o tabla en migración posterior.

---

## Índices en PostgreSQL

Para tablas grandes, preferir índices concurrentes.

Laravel puede requerir statement raw:

```php
DB::statement('CREATE INDEX CONCURRENTLY idx_orders_company_created ON orders (company_id, created_at DESC)');
```

Nota: `CREATE INDEX CONCURRENTLY` no puede ejecutarse dentro de una transacción normal.

---

## Backfill seguro

Reglas:

- Procesar por lotes.
- No cargar todo en memoria.
- Registrar progreso.
- Reejecutable.
- Idempotente.
- Pausable.

Ejemplo:

```php
Model::query()
    ->whereNull('new_column')
    ->orderBy('id')
    ->chunkById(200, function ($rows) {
        foreach ($rows as $row) {
            $row->update(['new_column' => computeValue($row)]);
        }
    });
```

---

## Cambios destructivos

Evitar directamente:

- Drop column.
- Rename column.
- Change type.
- Not null inmediato en tabla grande.
- Drop table.

Alternativas:

- Crear columna nueva.
- Copiar datos.
- Escribir doble por un tiempo.
- Leer de nueva con fallback.
- Eliminar viejo después.

---

## Rollback

Antes de migrar:

- ¿El rollback pierde datos?
- ¿Hay backup?
- ¿Hay feature flag?
- ¿La app vieja funciona con esquema nuevo?
- ¿La app nueva funciona con esquema viejo temporalmente?

---

## Checklist migración

- [ ] No modifiqué migración antigua.
- [ ] Evalué si es destructiva.
- [ ] Usé expand/contract si aplica.
- [ ] Evité locks largos.
- [ ] Índices grandes concurrentes considerados.
- [ ] Backfill por lotes.
- [ ] Rollback viable.
- [ ] Staging probado.
- [ ] Backup reciente.
- [ ] Feature flag considerado.
