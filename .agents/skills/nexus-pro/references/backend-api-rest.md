# Backend API & REST Reference

## Propósito

Este documento establece las directivas avanzadas para el diseño, estructuración y seguridad de APIs (RESTful y arquitecturas backend desacopladas) dentro de NEXUS-PRO. El backend debe actuar como una capa segura, agnóstica de presentación y con contratos de API estables.

---

## 🧭 Estándar RESTful y Diseño de Contratos

### Estructura de Endpoints
Los recursos expuestos deben usar nombres en plural y seguir los verbos HTTP correspondientes de manera semántica:
- `GET /api/v1/orders` - Lista de órdenes (paginada).
- `GET /api/v1/orders/{id}` - Detalle de una orden específica.
- `POST /api/v1/orders` - Crear orden.
- `PUT /api/v1/orders/{id}` - Reemplazo completo de una orden.
- `PATCH /api/v1/orders/{id}` - Modificación parcial de una orden.
- `DELETE /api/v1/orders/{id}` - Eliminación lógica o física de una orden.

### Versionado de API
- **Obligatorio**: Toda API pública o consumida por clientes desacoplados (como SPAs o móviles) debe estar versionada desde la URL (ej. `/api/v1/`).
- No introducir *breaking changes* en la estructura de respuesta de una versión activa. Si se requiere una reestructuración de propiedades, crear un nuevo namespace (`/api/v2/`).

---

## 📦 Capa de Negocio Avanzada: DTOs, Validación y Serializadores

Para desacoplar por completo la persistencia (modelos ORM) del transporte (HTTP):

```text
Request ➡️ FormRequest/Validator ➡️ DTO ➡️ Service ➡️ Repository ➡️ Model ➡️ Resource/Presenter ➡️ JSON Response
```

### 1. Data Transfer Objects (DTOs)
- Los controllers no deben pasar arrays asociativos crudos a los servicios.
- Usar clases de DTO autovalidadas (como clases con tipado estricto en TypeScript o Readonly Properties en PHP 8.2+) para estructurar la transferencia de datos entre la capa de entrada (HTTP/CLI) y la capa de servicios.

### 2. Serializadores / Presenters (API Resources)
- **Prohibido**: Retornar modelos de ORM crudos en las respuestas.
- Toda respuesta debe ser procesada por un serializador (ej. `JsonResource` en Laravel, `class-transformer` en NestJS) para:
  - Ocultar columnas internas innecesarias o sensibles (ej. `password_hash`, `deleted_at`).
  - Mapear nombres de columnas a camelCase si el cliente frontend lo exige.
  - Asegurar la consistencia de tipos (ej. castear strings de base de datos a flotantes o booleanos).

---

## 🔒 Autenticación y Control de Sesión

### JWT vs State Cookies
- **APIs Desacopladas/Móviles**: Usar tokens firmados criptográficamente (JWT) o tokens de base de datos tipo `Sanctum` almacenados en memoria segura del cliente.
- **Single Page Applications (SPA) en el mismo dominio**: Preferir autenticación basada en cookies con estado HTTP-Only y SameSite configurado a `Lax` o `Strict` para prevenir ataques CSRF e inyección XSS de robo de token.

---

## ⚡ Idempotencia en Acciones Críticas

Para evitar transacciones duplicadas por fallos de red o doble clic en la interfaz:

- **Idempotency-Key**: Exigir una cabecera `Idempotency-Key` (UUIDv4 generado por el cliente) en peticiones mutantes críticas (`POST` y `PATCH` de pagos, facturación o creación de registros únicos).
- **Mecanismo de Lock**:
  1. Al recibir la petición, verificar en Redis si la llave `idempotency:[key]` ya existe.
  2. Si existe y el proceso está en curso, retornar un código de estado `409 Conflict`.
  3. Si existe y terminó, devolver la respuesta cacheada directamente sin ejecutar lógica del backend.
  4. Si no existe, guardar la llave con un TTL de 24 horas y ejecutar el servicio.

---

## 🏗️ Hexagonal & Clean Architecture (DDD-Lite)

En proyectos complejos que demandan escalabilidad y baja dependencia de frameworks:

### Capa de Dominio (Domain)
- Contiene los Modelos de Dominio, Entidades puras y Value Objects. No depende de base de datos ni de frameworks.

### Capa de Aplicación (Application)
- Contiene los Casos de Uso (Use Cases), Interfaces de Repositorios, Servicios de Aplicación y DTOs de entrada. Orquesta las reglas de negocio.

### Capa de Infraestructura (Infrastructure)
- Contiene los Controllers HTTP, Comandos de Consola, Implementación de Repositorios (ORM), Clientes de APIs externas y adaptadores del framework.

---

## 🩺 Checklist de Entrega de APIs

Antes de dar por terminado un Endpoint o Módulo API:

- [ ] El endpoint cuenta con validación de tipo, longitud y formato en la entrada.
- [ ] La respuesta JSON está estandarizada bajo una estructura coherente (`data`, `meta`, `errors`).
- [ ] Se verifica autorización y ownership a nivel de recurso.
- [ ] Las consultas críticas están protegidas contra ataques de inyección y parametrizadas.
- [ ] Se documentó el endpoint en la especificación OpenAPI / Swagger.
- [ ] Las llamadas a métodos mutantes críticos son seguras ante ejecuciones duplicadas.
- [ ] No se exponen trazas de error internas en caso de excepción `500`.
