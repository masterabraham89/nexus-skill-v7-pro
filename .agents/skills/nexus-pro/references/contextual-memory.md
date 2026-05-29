# Memoria Evolutiva Contextual (Project Memory)

**NEXUS-PRO** debe aprender de sus propios errores y de las reglas específicas impuestas por el programador en el proyecto actual. Para ello, utilizará un archivo de memoria persistente llamado `.nexus-memory.md` en la raíz del proyecto.

## Flujo de Trabajo:
1. **Lectura Inicial:** Al iniciar cualquier tarea o recibir una nueva petición, Antigravity DEBE revisar si el archivo `.nexus-memory.md` existe en la raíz del proyecto (usando `list_dir` o `view_file`). Si existe, **sus reglas sobrescriben cualquier comportamiento por defecto**.
2. **Actualización Autónoma:** Si durante el desarrollo Antigravity comete un error, o el programador le da una corrección estructural (ej. *"Aquí no usamos UUIDs, usamos BIGINT"*, *"La tabla no tiene company_id, se llama tenant_id"*), Antigravity **DEBE editar o crear** el archivo `.nexus-memory.md` y agregar esta regla para no volver a equivocarse.
3. **Formato de la Memoria:**
   El archivo `.nexus-memory.md` debe estar en formato lista, directo y estricto.

### Ejemplo de `.nexus-memory.md`:
```markdown
# Nexus Contextual Memory for [Project Name]

- **Database:** La columna de aislamiento multi-tenant se llama `tenant_id`, NUNCA `company_id`.
- **Frontend:** Usamos `react-hook-form` con `zod`, no usar validación manual.
- **Tailwind:** Está prohibido usar colores hexadecimales quemados, siempre usar las clases de tema (ej. `bg-primary-500`).
```

Al hacer esto, el agente se vuelve hiper-personalizado a las necesidades del proyecto con el tiempo.
