# Tenant Isolation Reference

## Propósito

Este documento define reglas estrictas para aislamiento multiempresa. En NEXUS-PRO, todo dato privado debe pertenecer a una empresa o contexto autorizado. La regla central es: ningún usuario puede acceder, modificar, exportar, sincronizar o inferir datos de otra empresa.

## Regla principal

`companyId` siempre debe obtenerse desde token autenticado, sesión segura o contexto server-side. Nunca debe confiarse en `company_id` enviado desde frontend.

## Riesgos principales

- IDOR por IDs secuenciales.
- Filtros sin `company_id`.
- Jobs que procesan datos de varias empresas sin aislamiento.
- Exports que mezclan datos.
- Reportes agregados que filtran mal.
- Cache keys sin empresa.
- Policies que validan rol pero no ownership.
- Sync externo que no deduplica por empresa.

## Backend obligatorio

### Repositories

Todo método que lea datos privados debe filtrar por empresa:

```php
public function findByIdForCompany(int $id, int $companyId): ?Model
{
    return Model::query()
        ->where('id', $id)
        ->where('company_id', $companyId)
        ->first();
}
```

### Services

Los services deben resolver empresa desde usuario/token:

```php
$companyId = $this->tenantContext->companyIdFromAuthenticatedUser($user);
```

Prohibido:

```php
$companyId = $data['company_id'];
```

### Policies

Las policies deben validar:

- usuario autenticado;
- permiso;
- rol;
- empresa;
- ownership del recurso;
- estado del recurso si aplica.

## Cache multiempresa

Toda cache de datos privados debe incluir empresa:

```text
company:{companyId}:orders:list:{hashFilters}
```

Prohibido:

```text
orders:list
```

## Jobs multiempresa

Los jobs deben recibir `companyId` explícito desde contexto seguro al momento de encolarse. Deben filtrar todas sus queries por ese valor.

Reglas:

- No procesar datos globales sin scoping.
- No aceptar companyId desde payload público sin validación.
- Registrar companyId en logs.
- Aplicar locks por empresa cuando corresponda.

## Exports

Todo export debe:

- filtrar por empresa;
- validar permiso;
- auditar actor;
- generar archivo privado;
- usar URL temporal;
- evitar mezcla de tenants.

## Reportes

Los reportes deben filtrar por empresa antes de agregar datos. No basta con filtrar al final.

Correcto:

```sql
SELECT status, count(*)
FROM orders
WHERE company_id = :company_id
GROUP BY status;
```

## Tests obligatorios

Probar:

- Usuario empresa A no lee datos empresa B.
- Usuario empresa A no actualiza datos empresa B.
- Usuario empresa A no borra datos empresa B.
- Usuario no puede cambiar `company_id` por payload.
- Repository filtra por `company_id`.
- Job respeta `company_id`.
- Export respeta `company_id`.
- Reporte respeta `company_id`.
- Cache key incluye empresa.

## Checklist

- [ ] companyId viene desde token/contexto seguro.
- [ ] Repository filtra por empresa.
- [ ] Policy valida ownership.
- [ ] Job aísla empresa.
- [ ] Export aísla empresa.
- [ ] Reporte filtra antes de agregar.
- [ ] Cache key contiene companyId.
- [ ] Tests de aislamiento incluidos o recomendados.
