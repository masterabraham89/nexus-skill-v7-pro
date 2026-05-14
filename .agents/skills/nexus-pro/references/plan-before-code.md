# Plan Before Code Reference

## Propósito

Este documento obliga a Antigravity a planificar antes de modificar código. Su objetivo es reducir improvisación, cambios fuera de alcance, regresiones, duplicación de archivos y decisiones técnicas no verificadas.

## Regla principal

Antes de escribir código, Antigravity debe comprender el requerimiento, revisar el patrón existente y definir un plan mínimo de implementación.

No debe comenzar a crear archivos si todavía no sabe:

- qué flujo está modificando;
- qué dominio toca;
- qué archivos existentes participan;
- qué patrón arquitectónico ya usa el proyecto;
- qué riesgos existen;
- qué pruebas deben ejecutarse;
- qué impacto tiene en seguridad, datos, permisos y performance.

## Plan obligatorio

Antes de implementar, generar internamente o entregar al usuario cuando sea útil:

```text
1. Qué entendí del requerimiento.
2. Qué archivos o carpetas debo revisar.
3. Qué patrón existente debo respetar.
4. Qué archivos crearé o modificaré.
5. Qué riesgos técnicos existen.
6. Qué riesgos de seguridad existen.
7. Qué pruebas aplicaré o recomendaré.
8. Qué entregable final dejaré.
```

## Cuándo el plan debe ser visible al usuario

El plan debe mostrarse al usuario cuando:

- el cambio toque más de 3 archivos;
- el cambio afecte autenticación, autorización, pagos, facturación, roles, permisos, sync, imports, exports o migraciones;
- exista riesgo de breaking change;
- el usuario pida arquitectura, refactor o feature grande;
- haya ambigüedad técnica relevante;
- se detecte deuda fuera del alcance solicitado.

## Cuándo puede ser interno

Puede ser interno cuando:

- el bugfix sea pequeño;
- el cambio esté claramente delimitado;
- solo afecte texto, estilos menores o validaciones simples;
- el usuario pidió respuesta directa y el riesgo es bajo.

## Reglas anti-improvisación

- No inventar estructura si no se revisó el proyecto.
- No crear servicios duplicados si existe uno equivalente.
- No modificar contratos API sin identificar consumidores.
- No crear migraciones sin evaluar compatibilidad.
- No instalar dependencias sin justificar.
- No tocar archivos fuera del alcance sin reportarlo.
- No resolver bugs con parches superficiales si la causa raíz es identificable.

## Plantilla de plan técnico

```markdown
## Plan de implementación

### Entendimiento
[Resumen del requerimiento]

### Archivos a revisar
- [archivo/carpeta]

### Patrón existente
[Controller-Service-Repository, módulo React, job, policy, etc.]

### Cambios propuestos
- [crear/modificar archivo]

### Riesgos
- Seguridad:
- Datos:
- Performance:
- Compatibilidad:

### Pruebas
- Unit:
- Feature:
- E2E:
- Manual:
```

## Checklist

- [ ] Requerimiento entendido.
- [ ] Archivos existentes revisados.
- [ ] Patrón dominante identificado.
- [ ] Alcance delimitado.
- [ ] Riesgos identificados.
- [ ] Seguridad considerada.
- [ ] Performance considerada.
- [ ] Pruebas definidas.
