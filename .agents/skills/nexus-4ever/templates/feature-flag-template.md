# Feature Flag Template

## Registro de feature flag

```yaml
name: reports.new-dashboard.enabled
owner: engineering
status: beta
created_at: YYYY-MM-DD
review_at: YYYY-MM-DD
scope:
  - environment
  - company
  - role
kill_switch: true
description: "Activa el nuevo dashboard de reportes."
```

## Checklist

- [ ] Nombre claro.
- [ ] Owner.
- [ ] Fecha de revisión.
- [ ] Scope definido.
- [ ] Backend valida.
- [ ] Frontend no reemplaza permisos.
- [ ] Kill switch.
- [ ] Plan de eliminación.
