# Anti-Patch Policy (El Patrón de Refactorización Forzada)

Esta política es de cumplimiento obligatorio para la inteligencia artificial. Antigravity tiene ESTRICTAMENTE PROHIBIDO implementar "parches" rápidos (spaghetti code) que acumulen deuda técnica.

## 1. La Regla del Boy Scout (Refactor First)
Antes de añadir nueva funcionalidad a un archivo existente, Antigravity debe evaluar:
- ¿El archivo superará el límite de líneas recomendado?
- ¿Estoy añadiendo una nueva responsabilidad a esta clase/componente?

Si la respuesta es SÍ a cualquiera de las dos preguntas, Antigravity **DEBE obligatoriamente** extraer la lógica existente a un Service, Custom Hook, Helper o Trait **ANTES** de implementar la funcionalidad nueva.

## 2. Hard Limits (Límites Estrictos de Archivo)
Para asegurar la mantenibilidad a nivel Enterprise, ningún archivo debe convertirse en un "God Object".
- **Controladores:** Máximo 150 líneas.
- **Servicios:** Máximo 300 líneas.
- **Componentes React:** Máximo 150 líneas.
- **Custom Hooks:** Máximo 140 líneas.
Si la petición del usuario fuerza a superar estos límites, Antigravity debe crear módulos separados por defecto.

## 3. Prohibición del "Arrow Code"
Queda estrictamente prohibido el anidamiento profundo de bloques `if/else`. 
La IA debe aplicar el patrón de **Retorno Anticipado (Early Return)**. Validar los errores al inicio de la función y fallar rápido.

## 4. Single Source of Truth
No se debe duplicar la lógica de negocio. Si una regla (ej. cálculo de impuestos o validación de stock) existe en el backend, el frontend no debe recalcularla manual y redundantemente, sino consumir la respuesta estructurada del backend.

## 5. El Bloque "Nexus God Mode"
Antes de generar código para requerimientos complejos, la IA debe generar internamente un análisis respondiendo:
1. ¿Esto rompe el principio OCP (Open-Closed Principle)?
2. ¿Acopla el sistema o es agnóstico?
Solo si la arquitectura se mantiene limpia, la IA procede con el código.
