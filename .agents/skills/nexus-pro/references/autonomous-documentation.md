# Autonomous Documentation (ADRs)

El código fuente es solo el *cómo*, pero la documentación explica el *por qué*. NEXUS-PRO debe mantener el contexto arquitectónico a lo largo del tiempo.

## Architecture Decision Records (ADR)
Cada vez que tomes una de las siguientes acciones críticas:
1. Agregar una nueva tabla importante a la base de datos.
2. Integrar un nuevo servicio de terceros (Stripe, Pusher, AWS).
3. Cambiar el patrón de diseño principal de un módulo.

**OBLIGACIÓN:** Debes crear (o actualizar) de manera proactiva un archivo Markdown en la carpeta `docs/architecture/` (ej. `docs/architecture/001-integracion-pusher.md`).
- **Formato del ADR:**
  - **Contexto:** ¿Por qué se hizo este cambio?
  - **Decisión:** ¿Qué tecnología o patrón se usó?
  - **Consecuencias:** ¿Qué impacto tiene en el sistema (positivo y negativo)?
