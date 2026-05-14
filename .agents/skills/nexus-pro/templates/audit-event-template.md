# Audit Event Template

```php
$audit->record([
    'company_id' => $companyId,
    'actor_id' => $user->id,
    'action' => 'module.entity.action',
    'entity_type' => DomainModel::class,
    'entity_id' => (string) $model->id,
    'before_data' => $beforeSafeData,
    'after_data' => $afterSafeData,
    'metadata' => [
        'request_id' => $requestId,
    ],
    'ip_address' => request()->ip(),
    'user_agent' => request()->userAgent(),
    'request_id' => $requestId,
]);
```

## Checklist

- [ ] Sin secretos.
- [ ] Datos sensibles enmascarados.
- [ ] company_id.
- [ ] actor_id.
- [ ] action estándar.
- [ ] entity_type/entity_id.
- [ ] request_id.
