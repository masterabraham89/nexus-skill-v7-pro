# Changelog — NEXUS-PRO (nexus-skill-v7-pro-enterprise)

## 7.3-mobile-ux (Flutter-Style Native Mobile UX Engine)

La IA ahora construye interfaces móviles con calidad de aplicación nativa, aplicando la misma filosofía arquitectónica que Flutter (Scaffold, App Shell, Bottom Sheets, Thumb Zones) pero sobre tecnologías web (React, Tailwind CSS, PWA).

- **`mobile-ux-patterns.md` (NUEVA REFERENCIA CRÍTICA):** Cerebro de diseño móvil que le enseña al agente la anatomía correcta de cada tipo de aplicación para pantallas táctiles.
- **App Shell Container (Scaffold Web):** El agente siempre envolverá las vistas en un contenedor restringido (`max-w-md`, `h-[100dvh]`, `flex flex-col`) para evitar que la interfaz se estire grotescamente en pantallas grandes y se vea perfecta en móvil.
- **Arquetipos de UI por Plataforma:** El agente detecta el tipo de aplicación y aplica el patrón correcto automáticamente:
  - **POS / Catálogos Digitales:** Categorías superiores deslizables, grilla de productos compacta (`grid-cols-2`), carrito en Bottom Sheet inferior.
  - **E-commerce:** Carruseles táctiles con snap, barra inferior fija con total y botón de pagar.
  - **Paneles Administrativos:** Bottom Navigation Bar (prohibido Sidebar en móvil), tablas convertidas en tarjetas, filtros en Bottom Sheet.
  - **Formularios / Landing Pages:** Wizards paso a paso, botón de acción fijo al fondo de la pantalla (`mt-auto` o `fixed bottom-0`).
- **Ergonomía y Touch Targets:** Ningún elemento interactivo puede tener menos de `44px × 44px`. El agente aplica las "zonas calientes" del pulgar (Thumb Zone), colocando siempre las acciones principales abajo.
- **Bottom Sheets > Modales:** Prohibidos los modales centrados flotantes en vista móvil. Todo diálogo surge desde abajo.
- **Safe Areas:** Uso correcto de `env(safe-area-inset-bottom/top)` para no solapar controles con la barra gestual del iPhone o el notch de Android.
- **Micro-interacciones:** Estado `active:scale-95` en botones para feedback visual inmediato (equivalente al Ripple de Material Design en Flutter).
- **Nuevos Triggers:** El skill ahora se activa automáticamente con: `"diseñar vista móvil"`, `"aplicar app shell"`, `"mejorar ux móvil"`.
- **Nuevo Social Preview:** Banner promocional actualizado con el nuevo posicionamiento de Mobile UX nativo.

## 7.2-bug-resolution-engine (Motor Autónomo de Resolución SRE)

Conversión de la IA de un simple "reportador de bugs" a un **Ingeniero SRE Autónomo**:

- **Bug Resolution Engine (BRE):** Sistema de 5 fases (Triage, Protocolo por Nivel, Ciclo Universal, Guías de Solución y Checklist de Cierre) para diagnosticar y resolver errores de forma estructurada (`bug-resolution-engine.md`).
- **Protocolos de Severidad (P1-P4):** Playbooks de respuesta adaptados al tipo de fallo (desde Modo Emergencia con Hotfix hasta Triangulación de 3 Capas).
- **The 5 Whys (Causa Raíz):** Obligación algorítmica de documentar 3 a 5 niveles lógicos de diagnóstico antes de proponer código, atacando la causa estructural y no el síntoma.
- **Definition of Done (DoD) para Bugs:** 17 puntos de validación (arquitectura, testing, seguridad y documentación) que la IA debe completar para dar un fallo por cerrado.

## 7.1-diagnostics-sre (Modo Diagnóstico Inteligente)

Nueva capacidad SRE para análisis de incidentes:

- **Root Cause Analysis (RCA):** Nuevo protocolo (5 Whys) para diagnosticar estructuralmente incidentes complejos antes de proponer parches rápidos (`diagnostics-rca.md`).
- **Análisis de Logs & Stack Traces:** Reglas explícitas para interpretar errores de backend (Laravel) y frontend (React), ignorando el ruido del framework y ubicando el fallo exacto en el *User-Land Code* (`log-trace-analysis.md`).
- **Clasificación de Incidentes:** Matriz de triage (P1 a P4) para responder adecuadamente según la severidad (ej. modo de emergencia para caídas de base de datos) (`incident-classification.md`).
- **Visual Debugging:** Protocolo de análisis multimodal para inspeccionar capturas de pantalla de la UI rota, consolas del navegador y terminales (`visual-debugging.md`).
- **Nuevos Prompts:** `incident-triage-prompt.md` y `visual-bug-prompt.md` para iniciar rápidamente investigaciones forenses de código.

## 7.0-pro-enterprise (God Mode / Architecture Enforcer)

Actualización crítica de directivas de IA:

- **Anti-Patch Policy (Refactor-First):** Prohibido el parcheo ciego. Si el archivo supera el límite de tamaño, Antigravity DEBE refactorizar creando Servicios o Custom Hooks.
- **Evidence-Based Debugging:** Protocolo de triangulación de errores obligatorio. La IA debe verificar la existencia de variables/tablas (`view_file` o `grep`) antes de codificar soluciones y tiene un límite de 3 búsquedas para evitar bucles infinitos.
- **La Biblia de Buenas Prácticas:** Reglas estrictas aplicadas al tipo de archivo (Controllers sin dependencias masivas, Services stateless, Repositorios con eager loading, React Dumb Components).
- **TDD Strict Mode:** Obligación de crear pruebas automáticas (Test-First) antes de implementar funciones críticas de backend o frontend.
- **Escalabilidad de DB & Índices:** Prevención de *Full Table Scans* y obligatoriedad de índices en migraciones para columnas relacionales y frecuentes.
- **Seguridad Activa (Rate Limiting):** Todo endpoint crítico debe tener Throttling para prevenir ataques de fuerza bruta, DDoS y *cloud billing attacks*.
- **Task Handoff Protocol (No-Delegation Policy):** La IA tiene prohibido delegar comandos (migraciones, despliegues). Se introduce un "Motor de Flujos Dinámicos" que obliga a leer reglas de despliegue desde un `nexus-workflow.md` local para adaptarse a cualquier proyecto y erradicar la amnesia de contexto.
- **Enterprise Security (Zero-Trust & Fallbacks):** Bloqueo a credenciales hardcodeadas (usa .env obligatoriamente) e implementación de Graceful Degradation / Circuit Breakers para APIs de terceros.
- **Enterprise Performance (Payload Strictness):** Prohibición total de llamadas "SELECT *". Paginación obligatoria y Eager Loading estricto (Anti-N+1).
- **Autonomous Documentation (ADRs):** Autogeneración de *Architecture Decision Records* en `docs/architecture/` al agregar integraciones o cambiar esquemas pesados.
- **Self-Healing Audits (Pre-Commits de IA):** Obligación de ejecutar auto-escaneos (`npm run lint`, `php artisan test`) en consola y corregirse sola antes de declarar finalizada la tarea.
- **Estandarización de Git (Conventional Commits):** Los commits de la IA deben ser legibles, atómicos y cumplir con la semántica internacional.
- Eliminación global y estricta de términos de proyectos privados, estableciendo formalmente la marca blanca genérica **NEXUS-PRO**.

## 7.0-pro-enterprise (license — improvement #4)

Licencia definida:

- Licencia elegida: **Creative Commons Attribution-NonCommercial 4.0 (CC BY-NC 4.0)**.
- Archivo `LICENSE` creado en la raíz del repositorio.
- Tabla de casos de uso permitidos/no permitidos agregada al README.
- Badge de licencia funcional en el README.
- Sección de licencia en `SECURITY.md`.
- Uso libre para proyectos personales, educativos y open source.
- Uso comercial requiere licencia separada del autor.

## 7.0-pro-enterprise (rebrand)

Cambios de identidad:

- Skill renombrado a `NEXUS-PRO: Neural Expert for Unified eXtended Systems - Enterprise Edition`.
- Eliminadas referencias a proyectos privados.
- Skill convertido en recurso reutilizable para cualquier proyecto enterprise full stack.
- Agregada sección `target_projects` en frontmatter con los tipos de proyecto compatibles.
- Agregados tags: `saas`, `pos`, `e-commerce`, `pwa`, `capacitor`.
- Agregada sección "Proyectos para los que aplica" en el cuerpo del skill.

## 7.0-pro-enterprise (bilingual — improvement #2)

Soporte bilingüe:

- README.md reescrito en inglés como cara pública principal del repositorio.
- README.es.md creado como versión en español completa.
- CONTRIBUTING.md creado con guía de contribución pública.
- Triggers en inglés agregados al SKILL.md (`create endpoint`, `review security`, `audit code`, etc.).
- El skill ahora se activa tanto en español como en inglés.


## 7.0-pro-enterprise

Versión PRO del skill NEXUS-PRO.

Agregado:

- Plan Before Code.
- Scope Control.
- Tenant Isolation Testing.
- Feature Flags.
- Data Classification.
- Export Safety.
- File Upload Security.
- API Idempotency.
- Contract Testing.
- Dependency Governance.
- Environment Parity.
- Infrastructure Reference.
- Queue Reliability.
- Audit Trail Enterprise.
- Soft Delete / Hard Delete / Void Policy.
- Error Taxonomy.
- Documentation Generation.
- UX Failure States.
- Business Rules Registry.
- Definition of Done.
- Nuevos prompts reutilizables.
- Nuevas plantillas operativas.

## 6.0-enterprise-ultimate

Agregado:

- Execution Policies.
- Decision Trees.
- DDD-lite.
- Anti-Hallucination Rules.
- Migration Safety.
- API Standards.
- OpenAPI.
- Event-Driven Patterns.
- Observability Engineering.
- AI Coding Constraints.
- Architecture Review Mode.
- Production Readiness Gates.
- Prompt Contracts.
- Design System Rules.
- Git Strategy.

## 5.0-ultimate

Versión inicial enterprise con arquitectura Laravel/React/PostgreSQL, seguridad, performance, testing, CI/CD, monitoreo, accesibilidad, escalabilidad, i18n, PWA, disaster recovery y data privacy.
