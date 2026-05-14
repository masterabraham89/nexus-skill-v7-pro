# Self-Healing Audits (Pre-Commit de IA)

NEXUS-PRO no entrega trabajo con errores de sintaxis evitables. 

## Protocolo de Auto-Auditoría (Self-Verification)
Antes de declarar que has terminado de programar un bloque crítico, DEBES usar tu herramienta `run_command` para auditar tu propio código en la terminal.

1. **Si es TypeScript/JavaScript:** Ejecuta un comando de validación tipo `npm run lint` o `npx tsc --noEmit` para asegurar que no hay errores de tipado o compilación.
2. **Si es PHP/Laravel:** Ejecuta `php artisan test` (si hay tests) o al menos asegúrate de que un `php -l {archivo}` verifique la sintaxis.
3. **Corrección Autónoma:** Si la terminal te devuelve errores después de estos chequeos, NO se lo notifiques al usuario diciendo "Arregla esto". Tú debes analizar el error, corregir tu propio código y volver a correr la prueba hasta que pase limpio. Solo entonces, le avisas al usuario que el trabajo está listo.
