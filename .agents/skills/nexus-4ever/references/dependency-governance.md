# Dependency Governance Reference

## Propósito

Este documento define reglas para agregar, actualizar o eliminar dependencias. Una dependencia introduce riesgos de seguridad, peso, mantenimiento, licenciamiento y estabilidad.

## Regla principal

No instalar librerías nuevas sin justificación técnica.

## Antes de agregar una dependencia

Evaluar:

- ¿Ya existe una dependencia equivalente?
- ¿Se puede resolver con código simple?
- ¿Está mantenida?
- ¿Tiene vulnerabilidades?
- ¿Cuál es su licencia?
- ¿Cuánto pesa en frontend?
- ¿Tiene dependencias transitivas riesgosas?
- ¿Es compatible con stack actual?
- ¿Qué costo tendrá removerla?

## Frontend

Revisar:

- bundle size;
- tree shaking;
- imports parciales;
- soporte TypeScript;
- compatibilidad React;
- SSR/PWA si aplica;
- frecuencia de actualizaciones.

## Backend

Revisar:

- compatibilidad PHP/Laravel;
- CVEs;
- mantenimiento;
- integración con framework;
- configuración segura;
- soporte a testing.

## Auditoría

Ejecutar:

```bash
npm audit
composer audit
```

Complementar con revisión manual de dependencias críticas.

## Licencias

Evitar licencias incompatibles con el proyecto. Registrar dependencias con licencias especiales.

## Actualizaciones

- Actualizaciones de seguridad: prioridad alta.
- Major versions: revisar breaking changes.
- Lockfiles deben actualizarse de forma consistente.
- Ejecutar tests después de actualizar.

## Checklist

- [ ] Necesidad justificada.
- [ ] Alternativas evaluadas.
- [ ] Mantenimiento revisado.
- [ ] Vulnerabilidades revisadas.
- [ ] Licencia revisada.
- [ ] Peso frontend revisado.
- [ ] Lockfile actualizado.
- [ ] Tests ejecutados.
- [ ] Documentación actualizada si aplica.
