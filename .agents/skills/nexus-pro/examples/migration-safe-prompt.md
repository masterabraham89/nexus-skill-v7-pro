# Prompt: migración segura

Actúa usando el skill `nexus-pro`.

Necesito crear una migración para:

```text
[DESCRIBIR CAMBIO DE BASE DE DATOS]
```

## Reglas

- No modifiques migraciones antiguas.
- Evalúa si el cambio es destructivo.
- Usa expand/contract si aplica.
- Considera índices concurrentes en PostgreSQL si la tabla es grande.
- Define rollback.
- Define backfill por chunks de 200 si aplica.
- Considera feature flag si la app debe soportar esquema viejo y nuevo.
- Indica riesgos de lock.

## Entrega esperada

- Migración.
- Riesgos.
- Rollback.
- Plan de backfill.
- Checklist production readiness.
