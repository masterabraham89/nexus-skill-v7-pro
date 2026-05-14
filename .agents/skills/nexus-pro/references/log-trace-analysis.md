# Análisis de Logs y Stack Traces

Este documento define cómo NEXUS-PRO debe leer e interpretar trazas de error (Stack Traces) de Laravel, Node.js y React.

## 1. Lectura de Trazas de Backend (Laravel/PHP)
Cuando el usuario envía un bloque gigante de error de Laravel (`Illuminate\Foundation\Exceptions...`), NEXUS-PRO debe ignorar el "ruido" del framework y buscar el **Punto de Inyección de Código de Usuario (User-Land Code)**.

### Pasos obligatorios:
1. **Ignorar** las líneas que empiecen con `vendor/laravel/framework/`.
2. **Ubicar** la primera línea que apunte a `app/Http/Controllers/`, `app/Services/`, o `app/Repositories/`. Esa es casi siempre el origen de la falla.
3. **Extraer el mensaje exacto:** (ej. `SQLSTATE[23505]: Unique violation` o `Attempt to read property "id" on null`).
4. **Explicar en lenguaje humano:** *"El error ocurre en InvoiceService.php:45 porque estás intentando acceder a una propiedad de una variable que viene vacía (nula) desde la base de datos."*

## 2. Lectura de Errores de Frontend (React)
Si el error es de React (ej. `Uncaught TypeError: Cannot read properties of undefined (reading 'map')` o `Minified React error #185`):
1. **Identificar la asincronía:** En el 90% de los casos de propiedades "undefined", se debe a que el componente intentó renderizar la data ANTES de que terminara el fetch de la API.
2. **Revisar useEffects:** Buscar bucles infinitos por falta de dependencias correctas en el array.
3. **Proponer validación defensiva:** (ej. usar Optional Chaining `data?.items?.map` o renderizado condicional `if (isLoading) return <Loader />`).

## 3. Logs Silenciosos / Errores Lógicos
A veces no hay un "crash", pero los datos están mal (ej. un valor financiero se guarda como cero).
1. NEXUS-PRO debe solicitar la **Traza de Datos (Data Flow)**: pedir el DTO o FormRequest, el Service, y la consulta de Eloquent/TypeORM.
2. Buscar mutaciones de estado inesperadas o fallos de tipado estricto (ej. strings en lugar de enteros para cálculos de moneda).
