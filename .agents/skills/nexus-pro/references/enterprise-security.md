# Enterprise Security & Resilience (Zero Trust)

NEXUS-PRO debe operar bajo el principio de Confianza Cero (Zero Trust) y resiliencia máxima ante caídas.

## 1. Zero-Trust Secrets (Guardián de Credenciales)
**PROHIBICIÓN ABSOLUTA:** Jamás debes quemar (hardcode) tokens, contraseñas, claves API (Stripe, Pusher, AWS, etc.) ni URLs de bases de datos en el código fuente.
- **Obligación:** Utiliza siempre variables de entorno (ej. `process.env.STRIPE_KEY` o `env('STRIPE_KEY')`).
- **Plantillas:** Si agregas una nueva variable de entorno, DEBES agregarla inmediatamente al archivo `.env.example` con un valor en blanco o descriptivo para alertar al usuario de que debe configurarla.

## 2. Graceful Degradation & Circuit Breakers (Anticaídas)
**Regla de Resiliencia:** Ninguna aplicación debe crashear completamente si un servicio de terceros (APIs externas, pasarelas de pago, websockets) se cae.
- **Wrappers Seguros:** Toda llamada a un servicio de terceros DEBE estar envuelta en bloques `try/catch`.
- **Fallbacks:** Si una API falla, el sistema debe "degradarse amablemente". Por ejemplo: si el WebSocket falla, hacer un *fallback* a Polling; si la pasarela de pagos demora, registrar la orden como "Pendiente de Procesamiento" en lugar de arrojar un error HTTP 500 fatal al cliente.
