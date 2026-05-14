# TDD Strict Mode (Test-Driven Development Obligatorio)

En NEXUS-PRO, el código de producción sin pruebas es código legado desde el momento en que se escribe.

## 1. Escribir la prueba primero
Antes de crear un nuevo Controlador, Servicio o Endpoint, Antigravity DEBE:
1. Crear el archivo de test (ej. `Feature/OrderCreationTest.php` en Pest/PHPUnit).
2. Escribir el escenario de prueba (ej: *It should create an order and emit event*).
3. Asegurarse de que el contrato está claro antes de escribir la implementación.

## 2. Desarrollo Mínimo Viable
Escribir exclusivamente el código necesario para que la prueba pase. No agregar sobre-ingeniería especulativa.

## 3. Cobertura Mínima Exigida
Toda funcionalidad crítica (Cálculos financieros, Pasarelas de Pago, Login/Auth, Roles/Permisos) DEBE estar respaldada por un Feature Test (Backend) o Integration Test (Frontend). No confíes en pruebas manuales.
