# Anti-Hallucination Rules

## Propósito

Este documento evita que Antigravity invente contexto técnico inexistente. Es obligatorio para agentes de IA que modifican software real.

---

## Regla principal

Antigravity no debe asumir como existente ningún archivo, clase, columna, endpoint, permiso, relación, evento, tabla, job, hook o type sin verificarlo cuando el código del proyecto esté disponible.

---

## Prohibido inventar

- Nombres de columnas.
- Relaciones Eloquent.
- Permisos.
- Roles.
- Rutas API.
- Estructuras JSON.
- Variables de entorno.
- Services existentes.
- Hooks existentes.
- Tipos TypeScript.
- Estados de negocio.
- Índices DB.
- Jobs.
- Eventos.
- Policies.

---

## Obligatorio verificar

Antes de crear o modificar:

- Revisar migraciones para columnas.
- Revisar modelos para relaciones.
- Revisar rutas para endpoints.
- Revisar policies para permisos.
- Revisar services para patrones existentes.
- Revisar frontend modules para estructura.
- Revisar types para contratos.
- Revisar tests para comportamiento esperado.

---

## Cuando falte contexto

Antigravity debe declarar:

```text
Supuesto técnico: no encontré [X] en el contexto disponible. Propongo [Y] porque [razón]. Validar antes de producción.
```

No presentar supuestos como hechos.

---

## Naming conventions

Antes de nombrar nuevos archivos:

1. Buscar nombres similares.
2. Mantener convención del proyecto.
3. Evitar variantes innecesarias.
4. No duplicar conceptos.

Ejemplo:

Si existe `CustomerRepository`, no crear `ClientRepository` para el mismo dominio salvo que el negocio diferencie claramente Customer y Client.

---

## Validación de relaciones

No usar:

```php
$user->company->settings
```

sin verificar que:

- `company()` existe.
- `settings()` existe.
- Las relaciones cargan correctamente.
- Hay handling de null.

---

## Validación de permisos

No usar permisos como:

```php
$user->can('orders.manage')
```

sin verificar que ese permiso existe o que hay patrón equivalente.

---

## Validación de API contracts

No asumir que la API responde:

```json
{ "data": [] }
```

sin revisar Resources o controladores existentes.

---

## Checklist anti-hallucination

- [ ] Revisé migraciones/modelos antes de usar columnas.
- [ ] Revisé rutas antes de crear endpoints.
- [ ] Revisé permisos/policies antes de autorizar.
- [ ] Revisé estructura frontend antes de crear módulo.
- [ ] Revisé types antes de inventar interfaces.
- [ ] Declaré supuestos si faltó contexto.
- [ ] No dupliqué nombres ni conceptos existentes.
