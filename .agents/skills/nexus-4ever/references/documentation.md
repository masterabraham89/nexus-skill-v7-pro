# Documentation Reference

## Propósito

Este documento define cuándo y cómo actualizar documentación. Una feature no está completa si el equipo no puede entenderla, usarla, desplegarla, monitorearla o mantenerla.

## Documentos que deben actualizarse

Según el cambio:

- OpenAPI.
- README del módulo.
- Variables de entorno.
- Permisos nuevos.
- Jobs nuevos.
- Eventos nuevos.
- Migraciones importantes.
- Runbooks.
- Feature flags.
- Changelog.
- Guías de operación.

## README de módulo

Debe incluir:

```markdown
# Nombre del módulo

## Propósito
## Entidades principales
## Endpoints
## Permisos
## Jobs
## Eventos
## Reglas de negocio
## Variables de entorno
## Riesgos conocidos
## Pruebas
```

## Documentar permisos

Para cada permiso nuevo:

- nombre;
- descripción;
- roles que lo usan;
- acciones habilitadas;
- riesgos.

## Documentar jobs

Para cada job:

- cola;
- payload;
- idempotencia;
- retries;
- timeout;
- fallos comunes;
- reprocesamiento.

## Documentar migraciones

Para migraciones críticas:

- objetivo;
- impacto;
- tiempo estimado;
- rollback;
- compatibilidad;
- comandos.

## Checklist

- [ ] OpenAPI actualizado.
- [ ] README actualizado si aplica.
- [ ] Variables documentadas.
- [ ] Permisos documentados.
- [ ] Jobs documentados.
- [ ] Eventos documentados.
- [ ] Migraciones documentadas.
- [ ] Runbook actualizado si afecta producción.
