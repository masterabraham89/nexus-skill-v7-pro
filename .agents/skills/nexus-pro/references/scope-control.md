# Scope Control Reference

## Propósito

Este documento evita que Antigravity realice cambios innecesarios o riesgosos fuera del pedido del usuario. El objetivo es mantener control, previsibilidad y bajo riesgo en proyectos reales.

## Principio

El agente debe resolver el problema solicitado con el menor cambio seguro posible. La deuda técnica encontrada fuera del alcance debe reportarse como recomendación, no modificarse sin instrucción explícita.

## Reglas por tipo de solicitud

### Bugfix

Si el usuario pide corregir un bug:

- Identificar causa raíz.
- Cambiar solo los archivos necesarios.
- No hacer refactor masivo.
- No cambiar contratos API salvo que el bug esté en el contrato.
- Agregar prueba de regresión cuando sea posible.
- Reportar deuda técnica separada.

### Nueva funcionalidad

Si el usuario pide una feature:

- Crear estructura mínima completa.
- Respetar arquitectura existente.
- No rediseñar módulos vecinos.
- No cambiar permisos globales sin justificación.
- Documentar endpoints, permisos y datos nuevos.

### Refactor

Si el usuario pide refactor:

- Mantener comportamiento funcional.
- Evitar cambios de contrato no solicitados.
- Dividir por responsabilidad.
- Probar regresión.
- Registrar cambios de arquitectura.

### UI

Si el usuario pide UI:

- No cambiar backend salvo necesidad.
- No alterar contratos existentes.
- No introducir estado global sin necesidad.
- Mantener accesibilidad y responsive.

### Backend

Si el usuario pide backend:

- No modificar UI salvo que sea indispensable para validar flujo.
- No cambiar migraciones antiguas.
- No alterar datos productivos sin plan.

## Cambios prohibidos sin aprobación

- Reescribir módulo completo.
- Cambiar estructura global del proyecto.
- Instalar librerías nuevas.
- Cambiar versión de framework.
- Alterar contratos API públicos.
- Eliminar tests.
- Borrar migraciones antiguas.
- Modificar autenticación central.
- Cambiar permisos globales.
- Cambiar modelo multiempresa.
- Remover validaciones existentes.

## Manejo de deuda técnica detectada

Cuando se detecte deuda técnica fuera del alcance:

```markdown
## Deuda técnica detectada fuera del alcance

- Descripción:
- Riesgo:
- Archivo relacionado:
- Recomendación:
- Prioridad:
```

## Checklist

- [ ] El cambio responde exactamente al pedido.
- [ ] No se tocaron archivos innecesarios.
- [ ] No se cambió contrato sin justificar.
- [ ] No se hizo refactor no solicitado.
- [ ] La deuda externa fue reportada, no modificada.
- [ ] El cambio mínimo sigue siendo seguro.
