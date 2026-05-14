# Security Reference

## Propósito

Este documento define la revisión de seguridad obligatoria para NEXUS-4EVER. Todo cambio debe evaluarse antes de entregarse, especialmente si toca autenticación, autorización, datos privados, cargas de archivos, pagos, integraciones, sincronizaciones, reportes o administración.

---

## Principio de seguridad

La seguridad no debe agregarse al final. Debe estar integrada desde el diseño:

- Autenticación obligatoria.
- Autorización explícita.
- Ownership validado.
- Entrada validada.
- Salida controlada.
- Logs seguros.
- Secretos protegidos.
- Errores no reveladores.
- Permisos mínimos.
- Auditoría en acciones sensibles.

---

## OWASP Top 10 2024 — checklist operativo

Antigravity debe revisar los riesgos principales de aplicaciones web modernas, incluyendo:

1. Broken Access Control.
2. Cryptographic Failures.
3. Injection.
4. Insecure Design.
5. Security Misconfiguration.
6. Vulnerable and Outdated Components.
7. Identification and Authentication Failures.
8. Software and Data Integrity Failures.
9. Security Logging and Monitoring Failures.
10. Server-Side Request Forgery.

Además, debe cubrir los riesgos específicos exigidos por NEXUS-4EVER:

- SQL Injection.
- XSS.
- CSRF.
- IDOR.
- Brute Force.
- DDoS.
- Mass Assignment.
- SSRF.
- Insecure Deserialization.
- API Key exposure.
- Headers seguros.
- Logs seguros.
- Secrets management.
- Seguridad Laravel.
- Seguridad frontend.
- Seguridad auth.
- Roles y permisos.

---

## SQL Injection

### Riesgo

Ocurre cuando datos del usuario se concatenan en queries.

### Reglas

- Usar Eloquent o Query Builder.
- Usar bindings.
- Nunca concatenar strings en SQL.
- Validar filtros y columnas permitidas.
- Whitelist para sort fields.

Prohibido:

```php
DB::select("SELECT * FROM users WHERE email = '$email'");
```

Correcto:

```php
User::query()->where('email', $email)->first();
```

---

## XSS

### Riesgo

Inyección de scripts en UI.

### Reglas frontend

- No usar `dangerouslySetInnerHTML` salvo sanitización fuerte.
- Escapar contenido dinámico.
- Sanitizar HTML con librería confiable si se permite HTML.
- Validar URLs antes de renderizarlas.
- No insertar contenido del usuario en scripts.

### Reglas backend

- Validar entrada.
- Limitar longitud.
- Normalizar datos.
- Retornar texto como texto, no HTML.

---

## CSRF

### Riesgo

Acciones no autorizadas desde otro sitio.

### Reglas

- Para sesiones web, usar protección CSRF de Laravel.
- Para APIs con tokens, usar Bearer tokens correctamente.
- Configurar SameSite en cookies.
- No permitir métodos mutables sin autenticación.
- Validar origen cuando corresponda.

---

## IDOR

### Riesgo

Un usuario accede a recursos de otra empresa cambiando IDs.

### Regla obligatoria

Todo acceso a entidad privada debe filtrar por `company_id` desde token/contexto autenticado.

Correcto:

```php
$order = $this->orders->findByIdForCompany($orderId, $companyId);
```

Incorrecto:

```php
$order = Order::find($orderId);
```

---

## Brute Force

### Controles

- Rate limiting en login.
- Rate limiting en endpoints sensibles.
- Bloqueo temporal por intentos fallidos.
- Alertas por patrones sospechosos.
- CAPTCHA solo si es necesario.
- Logs de intentos fallidos sin exponer credenciales.

---

## DDoS

### Controles

- Rate limits por IP/usuario/token.
- CDN/WAF si aplica.
- Cache en endpoints públicos.
- Paginación obligatoria.
- Tamaño máximo de payload.
- Timeouts.
- Jobs para procesos pesados.
- Circuit breakers en integraciones externas.

---

## Mass Assignment

### Reglas Laravel

- Definir `$fillable` de forma explícita.
- Nunca usar payload completo sin filtrar.
- Usar `$request->validated()`.
- No permitir actualizar `role`, `company_id`, `is_admin`, `permissions` desde payload común.

Prohibido:

```php
$user->update($request->all());
```

Correcto:

```php
$user->update($request->validated());
```

---

## SSRF

### Riesgo

El servidor accede a URLs controladas por atacante.

### Controles

- Whitelist de dominios.
- Bloquear IPs privadas/locales.
- Validar esquema `https`.
- Timeouts cortos.
- No seguir redirects sin validación.
- No permitir URLs arbitrarias para webhooks, imágenes o imports.

---

## Insecure Deserialization

### Reglas

- No deserializar payloads no confiables.
- No usar `unserialize()` con datos externos.
- Preferir JSON validado.
- Firmar payloads sensibles.
- Validar estructura antes de procesar.

---

## API Key exposure

### Reglas

- Nunca colocar API keys privadas en frontend.
- Usar `.env`.
- No subir `.env` al repositorio.
- Rotar claves si se exponen.
- Usar secrets manager en CI/CD.
- Restringir claves por dominio/IP/permisos si el proveedor lo permite.
- No registrar claves en logs.

---

## Headers seguros

Configurar:

- `Content-Security-Policy`.
- `X-Frame-Options` o `frame-ancestors`.
- `X-Content-Type-Options: nosniff`.
- `Referrer-Policy`.
- `Permissions-Policy`.
- `Strict-Transport-Security` en HTTPS.
- Cookies `HttpOnly`, `Secure`, `SameSite`.

---

## Logs seguros

Los logs deben ayudar a investigar sin filtrar datos.

Permitido:

- ID de usuario.
- ID de empresa.
- ID de request.
- Acción.
- Entidad afectada.
- Resultado.
- Código de error.

Prohibido:

- Passwords.
- Tokens.
- API keys.
- Secretos.
- Datos bancarios completos.
- Payloads sensibles completos.
- Documentos personales sin necesidad.

---

## Seguridad Laravel

### Reglas

- Usar FormRequest.
- Usar Policies/Gates.
- Usar `$fillable`.
- Usar hashing con algoritmos seguros.
- Usar signed URLs cuando aplique.
- Validar uploads.
- Guardar archivos fuera de public cuando sean privados.
- Usar temporary URLs para archivos privados.
- Aplicar rate limiting.
- No exponer stack traces en producción.
- Desactivar debug en producción.
- Revisar permisos de storage/cache.

---

## Seguridad frontend

### Reglas

- No exponer secretos.
- No confiar en roles frontend como barrera final.
- No renderizar HTML no confiable.
- Validar tipos de archivo antes de subir.
- Manejar errores sin revelar datos internos.
- No imprimir tokens en consola.
- No dejar logs de debug en producción.
- Proteger rutas visualmente, pero depender del backend para autorización real.

---

## Seguridad en auth

### Controles

- Passwords hasheados.
- Rate limit en login.
- Revocación de tokens.
- Expiración de sesión/token.
- Reautenticación para acciones críticas.
- MFA si el negocio lo requiere.
- Logs de login/logout.
- Detección de dispositivos sospechosos si aplica.
- Respuesta genérica ante credenciales inválidas.

---

## Roles y permisos

### Reglas

- Usar principio de mínimo privilegio.
- Roles no deben hardcodearse en múltiples lugares.
- Permisos deben ser explícitos.
- Policies deben validar rol, permiso y ownership.
- Cambios de permisos deben auditarse.
- Usuarios sin empresa activa no deben operar datos de empresa.

---

## Checklist de seguridad obligatorio

Antes de entregar:

- [ ] Endpoint requiere autenticación si maneja datos privados.
- [ ] Policy/Gate aplicado.
- [ ] Ownership validado.
- [ ] `companyId` viene desde token/contexto autenticado.
- [ ] No se confía en `company_id` del cliente.
- [ ] FormRequest valida entrada.
- [ ] Límites de longitud definidos.
- [ ] Mass Assignment prevenido.
- [ ] SQL Injection prevenido.
- [ ] XSS revisado.
- [ ] CSRF considerado.
- [ ] IDOR prevenido.
- [ ] Rate limit aplicado si corresponde.
- [ ] Logs seguros.
- [ ] Errores no revelan internals.
- [ ] Secretos no expuestos.
- [ ] Headers seguros considerados.
- [ ] Dependencias revisadas.
