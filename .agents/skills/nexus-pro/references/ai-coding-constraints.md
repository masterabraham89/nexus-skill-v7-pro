# AI Coding Constraints

## Propósito

Este documento define restricciones específicas para agentes de IA que programan en proyectos reales.

---

## Antes de escribir código

Antigravity debe:

1. Leer estructura existente.
2. Identificar patrón dominante.
3. Revisar naming conventions.
4. Revisar rutas y módulos.
5. Revisar types y contratos.
6. Revisar tests existentes.
7. Revisar permisos.
8. Identificar archivos a tocar.

---

## No duplicar semánticamente

No crear:

- `UserService2`.
- `NewOrderRepository`.
- `customerApi.ts` si existe `customerService.ts`.
- `useFetchCustomers` si existe `useCustomers` con mismo propósito.

Primero extender o reutilizar patrón existente.

---

## No crear helpers genéricos innecesarios

Evitar:

- `utils/helpers.ts` gigante.
- `CommonService`.
- `GlobalFunctions`.
- `misc.ts`.

Crear utilidades por dominio o función clara.

---

## No introducir librerías sin aprobación técnica

Antes de agregar dependencia:

- Justificar.
- Evaluar alternativa nativa.
- Revisar mantenimiento.
- Revisar vulnerabilidades.
- Revisar tamaño.

---

## No ocultar errores

Prohibido:

```php
try {
    // ...
} catch (Throwable $e) {
    return null;
}
```

Debe registrar contexto seguro o lanzar excepción controlada.

---

## No silenciar TypeScript

Evitar:

- `any` innecesario.
- `// @ts-ignore`.
- Casts forzados sin explicación.
- Tipos inventados sin contrato backend.

---

## No hacer cambios cosméticos masivos

No reformatear archivos completos si el cambio es pequeño, salvo que el proyecto lo requiera. Reduce ruido en PR.

---

## Checklist AI

- [ ] Leí antes de escribir.
- [ ] No inventé contexto.
- [ ] No dupliqué archivos semánticos.
- [ ] No agregué dependencia innecesaria.
- [ ] No oculté errores.
- [ ] No usé `any` sin justificación.
- [ ] Cambio limitado al alcance.
