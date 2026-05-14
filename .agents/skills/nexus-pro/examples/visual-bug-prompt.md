# Visual Bug Investigation Prompt

Usa este prompt cuando subas una captura de pantalla de un error en el frontend, la consola del navegador, o un diseño roto.

---

Actúa usando el skill **nexus-skill-v7-pro-enterprise**.

**[CONTEXTO VISUAL]**
Te acabo de subir una captura de pantalla con un problema visual o un error de consola. Entra en **Modo Diagnóstico Visual**.

**LO QUE ESTÁ MAL:**
[Ej: "La tabla de usuarios aparece vacía a pesar de que hay datos en la BD" o "Me sale este texto rojo en la consola cuando hago click en Guardar".]

**TAREAS PARA TI (NEXUS-PRO):**
1. **OCR & Lectura:** Si hay un error en texto rojo en la consola (Network o Console tab), transcríbelo primero.
2. **Análisis de Capa:** Determina si el fallo es puramente CSS/Layout, si es un error de asincronía en React (ej. falta un loading state), o si parece ser que el payload del backend viene malformado.
3. **Hipótesis de Componente:** Si es un error de UI, no sugieras parches rápidos con estilos en línea. Sugiere la corrección estructural usando Tailwind o el patrón de componentes del Design System.
4. **Siguiente Paso:** Dime exactamente qué componente React o Request de Red necesitas ver en código para arreglarlo estructuralmente.
