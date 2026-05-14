# Feature Flags Reference

## Propósito

Este documento define cómo introducir funcionalidades con control operativo. Las feature flags permiten desplegar código sin activar funcionalidad para todos, reducir riesgo y facilitar rollback lógico.

## Cuándo usar feature flags

Usar flags cuando una funcionalidad:

- afecta flujos críticos;
- introduce migraciones progresivas;
- cambia contratos o comportamiento;
- requiere rollout por empresa;
- necesita beta privada;
- puede degradar performance;
- depende de integración externa;
- requiere kill switch.

## Tipos de flags

- Global: activa/desactiva en todo el sistema.
- Por ambiente: local, staging, production.
- Por empresa: rollout multiempresa.
- Por usuario/rol: beta o permisos graduales.
- Por porcentaje: rollout gradual.
- Kill switch: apagado inmediato.

## Reglas

- Toda flag debe tener nombre claro.
- Toda flag debe tener owner.
- Toda flag debe tener fecha de revisión.
- Toda flag temporal debe eliminarse cuando se estabilice.
- No llenar el código con flags eternas.
- No usar flags como reemplazo de permisos.
- Las flags no deben saltarse autorización.

## Naming

Formato recomendado:

```text
module.feature.behavior
billing.invoice-v2.enabled
inventory.sync-v2.enabled
reports.new-dashboard.enabled
```

## Backend

El backend debe validar flags antes de ejecutar comportamiento nuevo:

```php
if (! $this->features->enabled('billing.invoice-v2.enabled', $companyId)) {
    throw new FeatureDisabledException();
}
```

## Frontend

El frontend puede ocultar UI según flags, pero backend siempre debe validar.

```tsx
if (!features['reports.new-dashboard.enabled']) {
  return <LegacyReports />;
}
```

## Rollback lógico

Si la feature falla:

1. Desactivar flag.
2. Confirmar que tráfico vuelve a ruta estable.
3. Revisar logs y métricas.
4. Mantener código desplegado si no daña producción.
5. Corregir y reactivar gradualmente.

## Checklist

- [ ] Flag necesaria y justificada.
- [ ] Nombre claro.
- [ ] Owner definido.
- [ ] Fecha de revisión.
- [ ] Backend valida flag.
- [ ] Frontend solo oculta/muestra UI.
- [ ] Kill switch posible.
- [ ] Plan de eliminación.
- [ ] Métricas asociadas.
