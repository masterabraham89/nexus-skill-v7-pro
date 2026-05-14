# API Endpoint Template

## Archivos esperados

```text
routes/api.php
app/Http/Controllers/Api/V1/Domain/DomainController.php
app/Http/Requests/Domain/StoreDomainRequest.php
app/Http/Resources/DomainResource.php
app/Policies/DomainPolicy.php
app/Services/Domain/CreateDomainService.php
app/Repositories/DomainRepository.php
tests/Feature/Domain/CreateDomainTest.php
```

## Controller

```php
public function store(StoreDomainRequest $request, CreateDomainService $service): DomainResource
{
    $this->authorize('create', DomainModel::class);

    $model = $service->execute($request->user(), $request->validated());

    return new DomainResource($model);
}
```

## Service rules

- Recibe User y array validado.
- Resuelve companyId desde contexto seguro.
- Ejecuta negocio.
- Usa transaction si hay escrituras múltiples.
- No recibe Request.

## Repository rules

- Filtra por company_id.
- Contiene queries.
- No contiene negocio.

## Checklist

- [ ] Route versionada.
- [ ] FormRequest.
- [ ] Policy.
- [ ] Service.
- [ ] Repository.
- [ ] Resource.
- [ ] Test.
- [ ] OpenAPI.
