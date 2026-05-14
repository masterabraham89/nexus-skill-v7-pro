# Testing Reference

## Propósito

Este documento define las prácticas de testing para NEXUS-4EVER. Todo cambio crítico debe tener pruebas o, como mínimo, una estrategia de pruebas clara.

---

## Stack de testing

Backend:

- PHPUnit.
- Pest si el proyecto lo adopta.
- Feature tests Laravel.
- Unit tests para services.
- Tests de policies.
- Tests de FormRequest.
- Tests de jobs.

Frontend:

- Vitest.
- React Testing Library.
- Playwright o Cypress para e2e.
- axe-core para accesibilidad cuando aplique.

---

## Tipos de pruebas

### Unit tests

Validan lógica aislada:

- Services.
- Helpers puros.
- Cálculos.
- Reglas de negocio.
- Validadores.
- Transformadores.

### Feature tests

Validan flujos HTTP:

- Endpoints.
- Autenticación.
- Autorización.
- Validación.
- Respuestas JSON.
- Policies.
- Persistencia.

### E2E tests

Validan flujos completos:

- Login.
- CRUD crítico.
- Flujo de compra/orden.
- Sync.
- Reportes.
- Gestión de usuarios.
- Permisos.
- PWA/offline si aplica.

### Security tests

Validan:

- IDOR.
- Mass Assignment.
- SQL Injection básica.
- XSS.
- Rate limiting.
- Acceso sin permisos.
- Payloads inválidos.
- Uploads maliciosos.
- Tokens inválidos.

---

## PHPUnit

Pruebas backend recomendadas:

```php
public function test_user_cannot_access_order_from_another_company(): void
{
    $user = User::factory()->create(['company_id' => 1]);
    $order = Order::factory()->create(['company_id' => 2]);

    $this->actingAs($user)
        ->getJson("/api/orders/{$order->id}")
        ->assertStatus(404);
}
```

Reglas:

- Usar factories.
- Usar database transactions.
- Probar casos felices y fallidos.
- Probar authorization.
- Probar ownership.
- Probar validación.
- Probar side effects.

---

## Vitest

Usar para:

- Hooks.
- Funciones puras.
- Transformadores.
- Validadores frontend.
- Services con mocks.

---

## React Testing Library

Usar para probar comportamiento, no implementación.

Correcto:

- El usuario ve un mensaje.
- El usuario hace click.
- El formulario valida.
- Se muestra loading.
- Se muestra error.

Evitar:

- Probar estados internos.
- Depender de nombres de clases.
- Mocks excesivos que no representan uso real.

---

## Playwright o Cypress

Usar para e2e:

- Login.
- Navegación.
- Formularios críticos.
- Flujos de permisos.
- Acciones destructivas.
- Reportes.
- Carga de archivos.
- Offline/PWA si aplica.

---

## Cobertura mínima

Recomendación:

- Services críticos: 80% o más.
- Endpoints críticos: feature tests obligatorios.
- Policies: pruebas por rol y ownership.
- Frontend crítico: tests de componentes clave.
- E2E: flujos principales.

La cobertura no reemplaza criterio. Es mejor cubrir riesgos reales que perseguir porcentaje vacío.

---

## Flujos que deben probarse

- Login.
- Logout.
- Refresh/revocación de token.
- CRUD por empresa.
- Intento de acceso cruzado entre empresas.
- Roles y permisos.
- Imports.
- Upserts.
- Sync deduplicado.
- Transacciones con rollback.
- Errores de validación.
- Rate limits.
- Uploads.
- Reportes.
- Estados UI.

---

## Vulnerabilidades que deben probarse

- IDOR: cambiar ID en URL.
- Mass Assignment: enviar `company_id`, `role`, `is_admin`.
- SQL Injection: payloads en filtros.
- XSS: strings con scripts.
- CSRF si hay sesiones.
- Brute force: intentos repetidos.
- Upload malicioso.
- SSRF si hay URLs externas.
- Acceso sin token.
- Acceso con token sin permisos.
- Logs sin secretos.

---

## Tests para transactions

Debe probarse que si una operación falla:

- No quedan datos parciales.
- Se revierte auditoría asociada si corresponde.
- No se generan correlativos duplicados.
- No se envían notificaciones antes de confirmar operación.

---

## Tests para sync

Debe probarse:

- Datos duplicados.
- Reejecución del mismo lote.
- Chunks de 200.
- Fallo parcial.
- Retry.
- Idempotencia.
- Lock de concurrencia.
- Empresa correcta.

---

## Checklist testing

- [ ] Caso feliz probado.
- [ ] Validación probada.
- [ ] Authorization probada.
- [ ] Ownership probado.
- [ ] Error esperado probado.
- [ ] Transaction rollback probado.
- [ ] Sync idempotente probado.
- [ ] Frontend loading/error/empty probado.
- [ ] E2E considerado en flujo crítico.
- [ ] Vulnerabilidades principales revisadas.
