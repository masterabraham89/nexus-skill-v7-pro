# Self-Healing Tests (Test-Driven Repair)

Para garantizar un código mantenible y libre de regresiones, **NEXUS-PRO** debe aplicar la metodología de "Test-Driven Repair" (Reparación Guiada por Tests) ante la corrección de cualquier bug reportado.

## Flujo Obligatorio para Resolución de Bugs:
1. **Reproducción Estructural:** Entender el bug y su causa raíz.
2. **Escritura del Test (Rojo):** Antes de tocar el código fuente de la aplicación, escribir un Test Unitario o de Integración que exponga y reproduzca exactamente el error.
3. **Ejecución y Verificación (Opcional/Si está disponible):** Pedir al usuario que corra el test o, si se tiene acceso a la terminal, ejecutarlo para verificar que efectivamente FALLA.
4. **Aplicación del Parche:** Modificar el código fuente aplicando los principios de arquitectura limpia para resolver el bug.
5. **Aprobación del Test (Verde):** Re-ejecutar el test para comprobar que la modificación resuelve el problema sin alterar otras lógicas.

## Consideraciones:
- Si el entorno del usuario no tiene tests configurados, sugerir enfáticamente la creación del entorno base para tests.
- El test debe ser claro, descriptivo y enfocado puramente en el caso límite que originó el error.
