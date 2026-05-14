# La Biblia de Buenas Prácticas (Por Tipo de Archivo)

Esta es la referencia arquitectónica definitiva. Antigravity debe aplicar estrictamente estas reglas al generar o modificar código según su tipo.

## 🟢 BACKEND (Laravel / PHP)

### 1. Controllers (Controladores)
- **Responsabilidad:** Recibir peticiones HTTP, inyectar el Servicio y retornar una Respuesta.
- **Inyección Máxima:** 3 dependencias (Servicios). Si hay más, la arquitectura está acoplada.
- **Queries Directas:** 0. Prohibido usar `Model::where()` o `DB::table()` aquí.
- **Retorno Estricto:** Siempre deben retornar `API Resources` o `JsonResponses` estandarizadas. Jamás devolver un modelo de Eloquent puro para evitar fugas de datos (Mass Exposure).

### 2. Services (Capa Lógica)
- **Responsabilidad:** Orquestar reglas de negocio y delegar acceso a datos a los Repositorios.
- **Stateless (Sin Estado):** No debe tener variables de clase mutables.
- **Aislamiento HTTP:** Prohibido inyectar o acceder a `Request` o `session()`. Un servicio recibe un DTO puro, lo que permite que sea usado por la API o por un Comando Artisan indistintamente.

### 3. Repositories (Capa de Datos)
- **Responsabilidad:** Toda interacción con la base de datos (PostgreSQL/Redis).
- **Prohibición de N+1:** Uso obligatorio de `with()` para traer relaciones en bloque.
- **Selects Explícitos:** En lugar de `SELECT *`, usar `$query->select(['id', 'name'])` si la colección es mayor a 500 registros para ahorrar RAM.

---

## 🔵 FRONTEND (React / TypeScript)

### 1. Componentes UI (React)
- **Responsabilidad:** Solo dibujar en pantalla (Dumb Components).
- **Límite de Estado Local:** Máximo 2 hooks `useEffect` o `useState` por archivo. Si hay más, delegar a una máquina de estado o Custom Hook.
- **In-Render Functions:** Prohibido definir funciones pesadas directamente dentro del JSX (ej. `<button onClick={() => { ... }}>`). Usar `useCallback` o funciones referenciadas para evitar re-renders innecesarios.

### 2. Custom Hooks (Logica de UI)
- **Responsabilidad:** Encapsular la lógica compartida, llamadas a API y estados complejos.
- **Single Source of Truth:** Las llamadas a backend siempre deben encapsularse mediante herramientas como `SWR` o `React-Query`. No reinventar la rueda del fetch manual.
- **Tipado Estricto:** Prohibido devolver `any`. Se deben tipar los inputs y outputs obligatoriamente.

---

## 🟣 OPERACIONES Y DEPLOY

### 1. Migraciones de Base de Datos
- **Responsabilidad:** Evolucionar el esquema sin causar Downtime.
- **Cero Locks Largos:** Si se va a crear un índice en una tabla grande, usar `CONCURRENTLY` (PostgreSQL) para no bloquear la tabla y tirar la aplicación.
- **Safe Rollback:** Todo archivo de migración debe tener un método `down()` válido que limpie exactamente lo que se creó, salvo que implique pérdida de datos irreversibles (donde debe lanzar una excepción para avisar al humano).

### 2. Jobs (Colas y Procesos Pesados)
- **Responsabilidad:** Desacoplar tareas lentas del ciclo de vida HTTP.
- **Idempotencia Obligatoria:** Un job debe poder ejecutarse 5 veces por error de red y el resultado final debe ser idéntico al de 1 vez, sin duplicar registros.
- **Retries y Backoff:** Implementar políticas de reintento exponencial (`$backoff = [10, 30, 60]`).
