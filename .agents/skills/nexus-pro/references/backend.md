# Backend Reference

## Propósito

Este documento define las prácticas backend obligatorias para Laravel / PHP dentro de NEXUS-PRO. El backend debe ser seguro, modular, testeable, auditable y preparado para producción.

---

## Laravel best practices

### Reglas base

- Usar rutas API protegidas con middleware de autenticación cuando gestionen datos privados.
- Usar FormRequest para validar entrada.
- Usar Policies o Gates para autorización.
- Usar Services para negocio.
- Usar Repositories para queries.
- Usar Resources para respuestas.
- Usar Jobs para procesos pesados.
- Usar transacciones para operaciones críticas.
- Usar logs estructurados.
- Evitar lógica de negocio en controllers, models o migrations.

---

## Controllers

### Responsabilidad

Un controller debe ser delgado. Su trabajo es recibir la solicitud HTTP, delegar y responder.

Ejemplo correcto:

```php
public function store(StoreOrderRequest $request, OrderService $service)
{
    $user = $request->user();

    $order = $service->create(
        user: $user,
        data: $request->validated()
    );

    return new OrderResource($order);
}
```

### Prohibido

- `Model::where()` dentro del controller.
- Joins o query builder directo.
- Cálculos de negocio.
- Validación manual extensa.
- Autorización improvisada.
- Leer `company_id` desde el request como fuente de verdad.

---

## Services

### Responsabilidad

Los services contienen negocio. Deben recibir datos ya validados y usuarios autenticados cuando aplique.

Deben manejar:

- Reglas de negocio.
- Transacciones.
- Orquestación de repositories.
- Deducción de `companyId` desde usuario/token.
- Validaciones de dominio.
- Auditoría.
- Notificaciones.
- Jobs.
- Eventos.

Ejemplo:

```php
DB::transaction(function () use ($user, $data) {
    $companyId = $user->currentAccessToken()->abilities['company_id'] ?? $user->company_id;

    $this->policy->ensureCanCreateOrder($user, $companyId);

    return $this->orders->createForCompany($companyId, $data);
});
```

### Reglas obligatorias

- Toda operación que afecte múltiples tablas debe usar transaction.
- Todo error de negocio debe lanzar excepción controlada.
- Todo cambio sensible debe generar auditoría.
- Toda sincronización debe ser idempotente.
- Los upserts deben ejecutarse en chunks de 200.

---

## Repositories

### Responsabilidad

Los repositories encapsulan queries y persistencia.

Ejemplo:

```php
public function findByIdForCompany(int $id, int $companyId): ?Order
{
    return Order::query()
        ->where('id', $id)
        ->where('company_id', $companyId)
        ->first();
}
```

### Reglas

- Toda query multiempresa debe filtrar por `company_id`.
- Métodos deben tener nombres explícitos.
- Evitar retornar más columnas de las necesarias.
- Usar paginación para listados.
- Evitar N+1 con eager loading controlado.
- Usar locks cuando se generen correlativos.
- Usar upsert para sincronizaciones masivas.
- Usar chunks de 200 para imports y upserts.

---

## FormRequest

### Responsabilidad

Validar datos de entrada antes de llegar al controller.

Debe incluir:

- Reglas de tipo.
- Límites de longitud.
- Validaciones de formato.
- Reglas condicionales.
- Mensajes cuando sean necesarios.
- `authorize()` cuando la autorización sea simple.
- Policies cuando la autorización dependa de entidades.

Ejemplo:

```php
public function rules(): array
{
    return [
        'name' => ['required', 'string', 'max:120'],
        'email' => ['required', 'email', 'max:180'],
        'amount' => ['required', 'numeric', 'min:0', 'max:999999.99'],
    ];
}
```

---

## Sanctum

### Reglas

- Usar Sanctum para autenticación API.
- Asociar permisos/abilities al token cuando aplique.
- Obtener `companyId` desde token autenticado o contexto server-side.
- Nunca confiar en `company_id` enviado por frontend.
- Rotar tokens cuando haya riesgo.
- Revocar tokens en logout o cambios críticos.
- Aplicar expiración razonable según tipo de usuario.

---

## Gates y Policies

### Reglas

- Toda acción privada debe tener autorización.
- Toda lectura, creación, actualización o eliminación debe verificar ownership.
- Las policies deben considerar usuario autenticado, rol, permisos, empresa, estado del recurso y restricciones del negocio.

Ejemplo:

```php
public function update(User $user, Order $order): bool
{
    return $user->company_id === $order->company_id
        && $user->hasPermissionTo('orders.update');
}
```

---

## Jobs

### Uso correcto

Usar jobs para:

- Imports masivos.
- Envío de correos.
- Sincronizaciones externas.
- Procesamiento de imágenes.
- Reportes pesados.
- Notificaciones.
- Reintentos de integraciones.

### Reglas

- Jobs deben ser idempotentes.
- Deben tener retry/backoff.
- Deben registrar errores con contexto.
- No deben duplicar datos si se ejecutan dos veces.
- Deben usar locks cuando exista riesgo de concurrencia.
- No deben cargar datasets gigantes en memoria.

---

## Transactions

### Cuándo usar

- Creación de registros relacionados.
- Actualización de inventario.
- Generación de correlativos.
- Pagos.
- Facturación.
- Sincronización crítica.
- Cambios de estado complejos.
- Auditoría vinculada a operación principal.

### Regla

Toda transacción debe tener rollback natural mediante excepción. No capturar errores para ocultarlos.

Correcto:

```php
return DB::transaction(function () use ($data) {
    $order = $this->orders->create($data);
    $this->audit->record('order.created', $order);
    return $order;
});
```

---

## Manejo de errores

### Reglas

- Usar excepciones de dominio.
- No devolver trazas internas.
- Mapear errores esperados a respuestas HTTP claras.
- Registrar errores inesperados con contexto seguro.
- No exponer SQL, rutas internas, tokens o payloads sensibles.

Códigos recomendados:

- 400: request inválido.
- 401: no autenticado.
- 403: no autorizado.
- 404: recurso no encontrado o no perteneciente.
- 409: conflicto de negocio.
- 422: validación.
- 429: rate limit.
- 500: error inesperado.

---

## Logs

### Deben incluir

- `request_id`.
- `user_id`.
- `company_id`.
- Acción.
- Resultado.
- Duración.
- Entidad afectada.
- Código de error si aplica.

### Nunca incluir

- Passwords.
- Tokens.
- API keys.
- Secretos.
- Datos bancarios completos.
- Documentos sensibles innecesarios.
- Payload completo si contiene PII.

---

## Chunks

### Regla obligatoria

Upserts, imports y sincronizaciones deben ejecutarse en chunks de 200.

Ejemplo:

```php
collect($rows)
    ->chunk(200)
    ->each(function ($chunk) {
        Product::upsert(
            $chunk->toArray(),
            ['external_id', 'company_id'],
            ['name', 'price', 'updated_at']
        );
    });
```

---

## Antifraude

Aplicar controles cuando existan operaciones sensibles:

- Rate limiting.
- Auditoría.
- Detección de duplicados.
- Validación de IP/dispositivo si aplica.
- Revisión de cambios sospechosos.
- Bloqueo temporal ante intentos repetidos.
- Alertas por actividad anómala.
- Verificación de ownership.
- Logs no repudiables.

---

## Correlativos

Los correlativos deben generarse de manera segura ante concurrencia.

Opciones aceptadas:

- Secuencias PostgreSQL.
- Tabla de correlativos con lock transaccional.
- `lockForUpdate()` dentro de transaction.
- UUID/ULID cuando el negocio lo permita.

Prohibido:

- `max(number) + 1` sin lock.
- Generar correlativos en frontend.
- Confiar en valores enviados por cliente.

---

## Notificaciones

Las notificaciones deben ser desacopladas:

- Usar Notifications de Laravel.
- Usar Jobs para envío.
- Registrar estado de entrega si es crítico.
- Evitar bloquear request principal.
- No enviar datos sensibles innecesarios.
- Tener plantilla versionada cuando aplique.

---

## Checklist backend

Antes de entregar:

- [ ] Controller sin queries.
- [ ] FormRequest creado.
- [ ] Service con negocio.
- [ ] Repository con queries.
- [ ] Policy/Gate aplicado.
- [ ] `companyId` desde token/contexto autenticado.
- [ ] Ownership validado.
- [ ] Transaction aplicada si corresponde.
- [ ] Rollback garantizado por excepción.
- [ ] Upsert en chunks de 200.
- [ ] Sync deduplicado.
- [ ] Logs seguros.
- [ ] Errores controlados.
- [ ] Tests recomendados o implementados.
