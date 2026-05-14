# Prompt de nueva funcionalidad para Antigravity

Actúa usando el skill `nexus-skill-v7-pro-enterprise`.

Necesito implementar una nueva funcionalidad en el proyecto actual siguiendo arquitectura enterprise.

## Funcionalidad

Describe aquí la funcionalidad:

```text
[DESCRIBIR FUNCIONALIDAD]
```

## Reglas obligatorias

Backend:

- Laravel / PHP.
- Arquitectura `Controller -> Service -> Repository`.
- FormRequest para validación.
- Policy/Gate para autorización.
- Resource para respuesta.
- `companyId` desde token/contexto autenticado.
- Verificación de ownership.
- Transacciones con rollback si hay múltiples escrituras.
- Upserts en chunks de 200 si hay procesos masivos.
- Deduplicación si hay sincronización.
- Jobs para procesos pesados.
- Logs seguros.

Frontend:

- React / TypeScript.
- Módulo por dominio.
- Components sin fetch directo.
- Hooks sin JSX.
- Services para API.
- Types para contratos.
- Modales independientes.
- `useSWR` o `react-query`.
- Cleanup en `useEffect`.
- Loading, error, empty y success states.
- Accesibilidad básica.

## Seguridad

Antes de entregar, revisa:

- Autenticación.
- Autorización.
- Ownership.
- Validación de entrada.
- IDOR.
- Mass Assignment.
- XSS.
- SQL Injection.
- CSRF si aplica.
- Rate limiting si aplica.
- Logs seguros.
- Secrets.

## Performance

Revisa:

- Índices.
- Paginación.
- N+1.
- Cache.
- Jobs.
- Bundle size.
- Lazy loading.
- Render performance.

## Entrega esperada

1. Diseño técnico breve.
2. Archivos a crear/modificar.
3. Implementación.
4. Pruebas recomendadas.
5. Checklist de seguridad.
6. Checklist de performance.
7. Riesgos o supuestos.
