# Data Classification Reference

## Propósito

Este documento clasifica los datos por sensibilidad para definir controles de acceso, logging, retención, cifrado, exportación y auditoría.

## Niveles de clasificación

### Public

Datos destinados a ser públicos.

Ejemplos:

- Nombre público de una empresa.
- Catálogo público.
- Contenido marketing.

Controles:

- Puede cachearse públicamente.
- Puede estar en CDN.
- No requiere auditoría individual salvo cambios administrativos.

### Internal

Datos internos no sensibles.

Ejemplos:

- Configuraciones operativas no críticas.
- Métricas agregadas.
- Estados generales.

Controles:

- Acceso autenticado.
- Logs permitidos con moderación.
- No exponer públicamente.

### Confidential

Datos privados del negocio.

Ejemplos:

- Órdenes.
- Inventario.
- Clientes.
- Reportes.
- Documentos comerciales.

Controles:

- Autorización y ownership.
- No logs completos.
- Exports auditados.
- Cache privada por empresa.

### Sensitive Personal Data

Datos personales sensibles o identificables.

Ejemplos:

- DNI.
- RUC asociado a persona natural.
- Teléfono.
- Dirección.
- Email personal.

Controles:

- Minimización.
- Retención definida.
- Logs enmascarados.
- Acceso limitado.
- Auditoría.

### Financial Data

Datos financieros o monetarios.

Ejemplos:

- Facturas.
- Pagos.
- Saldos.
- Montos.
- Cuentas.

Controles:

- Auditoría estricta.
- No hard delete salvo política.
- Transacciones.
- Integridad fuerte.
- Logs seguros.

### Authentication Secrets

Credenciales y secretos.

Ejemplos:

- Passwords.
- Tokens.
- API keys.
- Refresh tokens.
- Secrets CI/CD.

Controles:

- Nunca loggear.
- Nunca exportar.
- Cifrar/hashear según tipo.
- Rotar.
- Acceso mínimo.

## Reglas por clasificación

| Clasificación | Logs | Export | Cache | Auditoría | Retención |
|---|---|---|---|---|---|
| Public | Permitido | Permitido | Pública | Baja | Flexible |
| Internal | Limitado | Controlado | Privada | Media | Definida |
| Confidential | Enmascarado | Auditado | Por empresa | Alta | Definida |
| Sensitive Personal Data | Enmascarado | Restringido | Cuidado extremo | Alta | Legal |
| Financial Data | Enmascarado | Auditado | Evitar si crítico | Muy alta | Legal/contable |
| Authentication Secrets | Prohibido | Prohibido | Prohibido | Acceso auditado | Rotación |

## Checklist

- [ ] Campo nuevo clasificado.
- [ ] Logs revisados.
- [ ] Export permitido o restringido.
- [ ] Retención definida.
- [ ] Acceso por rol/permiso.
- [ ] Auditoría definida.
- [ ] Cache segura.
