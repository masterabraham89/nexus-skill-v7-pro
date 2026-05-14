# Incident Triage & RCA Prompt

Usa este prompt cuando tu sistema tenga una caída grave, un error 500, o un fallo en producción.

---

Actúa usando el skill **nexus-skill-v7-pro-enterprise**.

**[CONTEXTO DE INCIDENTE]**
Acabamos de detectar un error grave en la aplicación. Entra en **Modo Diagnóstico (SRE)**. No escribas código de solución hasta que estemos de acuerdo en la causa raíz.

**Gravedad Estimada:** [P1 Crítico / P2 Alto]
**Ambiente:** [Producción / Staging / Desarrollo]

**SÍNTOMAS:**
[Describe brevemente qué se rompió. Ej: "Los usuarios no pueden completar el pago con tarjeta. La API devuelve 500."]

**TRAZA DE ERROR / LOGS (Pegar aquí):**
```text
[Pega el error de tu consola, servidor o Log aquí]
```

**TAREAS PARA TI (NEXUS-PRO):**
1. **Contención:** Si esto es Producción, dime inmediatamente si sugieres hacer rollback o apagar alguna feature flag.
2. **Análisis:** Extrae la línea exacta del código de usuario (ignorando el framework) donde revienta.
3. **5 Whys (Causa Raíz):** Aplica la metodología de los 5 Porqués para encontrar por qué falló estructuralmente.
4. **Hipótesis:** Dame 2 hipótesis de por qué ocurre esto (capa de BD, capa de Lógica, o Infraestructura).
5. **Validación:** ¿Qué archivo o línea de código específico necesitas que te muestre para confirmar tu hipótesis principal? No asumas el código, pídemelo.
