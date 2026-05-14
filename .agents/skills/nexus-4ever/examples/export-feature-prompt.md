# Prompt de export seguro para Antigravity

Actúa usando el skill `nexus-4ever`.

Necesito crear o revisar un export de datos para:

```text
[DESCRIBIR EXPORT]
```

Requisitos:

- Validar permiso específico.
- Filtrar por companyId desde contexto autenticado.
- Definir allowlist de columnas.
- Limitar rango de fechas y cantidad de filas.
- Procesar por job si es pesado.
- Guardar en storage privado.
- Generar URL temporal.
- Auditar generación y descarga.
- Prevenir CSV Injection.
- Enmascarar datos sensibles si aplica.

Entrega:

- Diseño técnico.
- Archivos a crear/modificar.
- Código.
- Tests.
- Checklist de export safety.
