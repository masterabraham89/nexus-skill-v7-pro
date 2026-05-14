# DDD-lite Reference

## Propósito

DDD-lite permite ordenar dominios complejos sin sobrearquitectura. NEXUS-4EVER no exige DDD puro, pero sí recomienda aplicar conceptos de dominio cuando el proyecto crece.

---

## Cuándo usar DDD-lite

Usar cuando existan:

- Reglas de negocio complejas.
- Múltiples estados.
- Procesos con auditoría.
- Módulos grandes.
- Integraciones.
- Roles y permisos sofisticados.
- Entidades con invariantes.
- Operaciones transaccionales.

No usar para CRUD trivial.

---

## Bounded Contexts

Separar dominios por lenguaje y responsabilidad:

```text
app/Domains/
├── Auth/
├── Companies/
├── Inventory/
├── Orders/
├── Billing/
├── Reporting/
└── Sync/
```

Cada contexto puede contener:

```text
Actions/
DTOs/
Events/
Models/
Policies/
Repositories/
Services/
ValueObjects/
```

---

## Use Cases / Actions

Un use case representa una acción del negocio:

- `CreateOrderAction`
- `ApproveInvoiceAction`
- `SyncProductsAction`
- `AssignUserRoleAction`

Debe tener un método claro, normalmente `execute()` o `handle()`.

---

## Value Objects

Usar Value Objects para conceptos que requieren validación o formato:

- Money.
- RUC.
- DNI.
- Email.
- PhoneNumber.
- DateRange.
- Percentage.

Ejemplo conceptual:

```php
final class Money
{
    public function __construct(
        public readonly string $currency,
        public readonly string $amount
    ) {
        if (! preg_match('/^\d+(\.\d{2})$/', $amount)) {
            throw new InvalidArgumentException('Monto inválido.');
        }
    }
}
```

---

## Domain Events

Usar eventos para comunicar hechos del negocio:

- `OrderCreated`.
- `InvoiceApproved`.
- `UserRoleChanged`.
- `InventorySynced`.

Los eventos deben nombrarse en pasado porque representan algo que ya ocurrió.

---

## Domain Services

Usar cuando una regla no pertenece naturalmente a una entidad específica:

- Cálculo de comisión.
- Validación de cupo.
- Generación de correlativo.
- Asignación de prioridad.

---

## Aggregates

Usar aggregates cuando se requiere proteger invariantes:

- Orden con líneas.
- Factura con pagos.
- Empresa con configuración fiscal.
- Usuario con roles/permisos.

Regla: modificar el aggregate desde un use case, no desde múltiples puntos dispersos.

---

## Relación con Controller-Service-Repository

DDD-lite puede coexistir:

```text
Controller -> FormRequest -> Policy -> UseCase/Service -> Repository -> DB
```

No introducir DDD si solo complica. La prioridad es claridad.

---

## Checklist DDD-lite

- [ ] El dominio tiene nombre claro.
- [ ] Las reglas viven en services/use cases.
- [ ] Los Value Objects validan conceptos críticos.
- [ ] Los eventos representan hechos del negocio.
- [ ] No hay lógica crítica dispersa en controllers.
- [ ] No hay sobrearquitectura para CRUD simple.
