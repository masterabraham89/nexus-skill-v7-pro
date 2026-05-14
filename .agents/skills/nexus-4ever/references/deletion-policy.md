# Deletion Policy Reference

## Propósito

Este documento define cuándo usar soft delete, hard delete, anulación o retención. Borrar datos sin política puede romper auditoría, cumplimiento, reportes y trazabilidad.

## Principio

No todo debe eliminarse físicamente. Entidades legales, financieras o auditables normalmente deben anularse o desactivarse, no borrarse.

## Estrategias

### Soft delete

Usar para:

- usuarios;
- clientes;
- productos;
- registros operativos recuperables;
- entidades con historial.

### Hard delete

Usar para:

- archivos temporales;
- sesiones expiradas;
- tokens revocados antiguos;
- caches;
- datos efímeros.

### Anulación

Usar para:

- facturas;
- documentos legales;
- transacciones financieras;
- órdenes cerradas si el negocio requiere trazabilidad.

### Desactivación

Usar para:

- empresas;
- usuarios;
- integraciones;
- productos no vigentes;
- configuraciones.

## Reglas

- Toda eliminación sensible debe auditarse.
- Toda eliminación debe validar permiso.
- Toda eliminación debe validar ownership.
- No eliminar registros financieros sin política explícita.
- No borrar datos personales si existe obligación legal de retención, salvo procedimiento definido.
- Aplicar privacy deletion cuando corresponda.

## Cascadas

Evitar cascadas destructivas no revisadas. Preferir manejo explícito en service.

## Restauración

Si se usa soft delete, definir:

- quién puede restaurar;
- cuánto tiempo se conserva;
- qué relaciones se restauran;
- qué auditoría se registra.

## Checklist

- [ ] Tipo de eliminación definido.
- [ ] Permiso validado.
- [ ] Ownership validado.
- [ ] Auditoría incluida.
- [ ] Cascadas revisadas.
- [ ] Retención considerada.
- [ ] Restauración definida si aplica.
- [ ] Privacidad considerada.
