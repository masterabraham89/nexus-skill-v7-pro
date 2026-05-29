# Generación Autónoma de ADRs (Architecture Decision Records)

A fin de mantener la documentación del proyecto actualizada y estructurada sin fricción para el programador, **NEXUS-PRO** generará registros de decisiones arquitectónicas (ADR) de forma autónoma.

## Cuándo generar un ADR:
Antigravity debe crear automáticamente un archivo Markdown de ADR en `docs/architecture/decisions/` (o sugerir crearlo si la carpeta no existe) cuando realice:
- La creación de un módulo o microservicio completamente nuevo.
- La instalación o adopción de una librería clave (ej. Zustand, Sanctum, SWR, Redis).
- Un refactor estructural significativo.
- Un cambio en la arquitectura de base de datos (desnormalizaciones por rendimiento, nuevas tablas pivote complejas).

## Formato del ADR:
El archivo debe nombrarse como `YYYY-MM-DD-titulo-de-la-decision.md` y contener:

```markdown
# [Título corto de la decisión]

## Contexto y Problema
[Descripción técnica de por qué se requiere tomar una decisión o por qué el código anterior ya no era suficiente].

## Alternativas Consideradas
- [Alternativa 1]: [Por qué se descartó]
- [Alternativa 2]: [Por qué se descartó]

## Decisión Tomada
[Descripción técnica de la solución elegida (Framework, patrón de diseño, etc.)].

## Consecuencias (Impacto)
- Positivas: [Mejora de performance, mantenibilidad, etc.]
- Negativas/Trade-offs: [Curva de aprendizaje, dependencia externa, etc.]
```

**Instrucción:** El agente debe realizar esto *después* de codificar y como parte de la entrega de la tarea, anunciando: *"He generado un ADR documentando esta decisión en `docs/architecture/decisions/`"*.
