# Prompt de corrección de bug para Antigravity

Actúa usando el skill `nexus-pro`.

Necesito corregir el siguiente bug sin introducir regresiones:

## Bug

```text
[DESCRIBIR BUG]
```

## Contexto

```text
[PEGAR ERROR, LOG, RUTA, ARCHIVOS O PASOS PARA REPRODUCIR]
```

## Instrucciones

1. Reproduce mentalmente el flujo antes de modificar.
2. Identifica causa raíz, no solo síntoma.
3. Revisa si el bug está en frontend, backend, base de datos, permisos, cache o integración.
4. Aplica el cambio mínimo seguro.
5. Respeta arquitectura:
   - Controller sin DB.
   - Service con negocio.
   - Repository con queries.
   - Components sin fetch directo.
   - Hooks sin JSX.
6. Valida seguridad:
   - Auth.
   - Ownership.
   - Authorization.
   - Validation.
   - Mass Assignment.
   - IDOR.
7. Valida performance:
   - N+1.
   - Query lenta.
   - Render innecesario.
   - Cache inconsistente.
8. Agrega o sugiere prueba de regresión.

## Entrega esperada

Devuélveme:

- Causa raíz.
- Archivos modificados.
- Solución aplicada.
- Riesgos revisados.
- Prueba de regresión sugerida.
- Checklist de seguridad.
- Checklist de performance.
