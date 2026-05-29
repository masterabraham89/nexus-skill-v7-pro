# Ejemplo de Prompt para Investigación de Errores (Internet Research)

Cuando Antigravity/Nexus detecte un error que requiera investigación (por ejemplo, errores de conexión, incompatibilidad de versiones o fallos no documentados), usará internamente un prompt similar a este para consultar a la web (vía `perplexity-ask` o `search_web`).

---

**Prompt Interno del Agente:**

```text
Actúa como un ingeniero SRE de nivel experto especializado en [Framework/Tecnología involucrada].

Se ha encontrado el siguiente error en producción/desarrollo:
"[Mensaje de error exacto extraído del log o stack trace]"

Entorno:
- Lenguaje: [PHP 8.2 / Node 20 / etc.]
- Framework: [Laravel 11 / React 19 / etc.]
- Paquete/Librería afectada: [Nombre y Versión]

Tu tarea es investigar este error en internet (GitHub issues, documentación oficial, StackOverflow) y proporcionar:
1. **Causa Raíz:** Por qué ocurre este error exactamente a nivel estructural o de infraestructura.
2. **Solución Profesional:** La solución aceptada y recomendada por la comunidad para resolverlo definitivamente. Si requiere un cambio de configuración, proporciona los pasos.
3. **Workaround (Si aplica):** Si es un bug abierto, proporciona la mitigación más segura.
4. **Validación:** Confirma la fuente de la solución (ej. "Mencionado en el issue #45 del repo oficial").

No inventes código. Basa tu respuesta estrictamente en los resultados de tu investigación.
```

---

### Lo que el Usuario/Programador debe enviar a Nexus:
Para activar este flujo de investigación profunda, el programador solo necesita escribir uno de los triggers o enviar la captura/log de forma natural:

**Ejemplos de input del usuario:**
- `"investigar error en internet: [Pega aquí el log del error]"`
- `"Tengo este fallo de acoplamiento al actualizar Laravel, ¿puedes buscar cómo solucionarlo? [Pega el error]"`
- `"search internet for solution para este connection timeout en PostgreSQL"`
