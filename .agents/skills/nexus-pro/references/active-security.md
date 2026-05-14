# Seguridad Activa (Rate Limiting y Throttling)

En NEXUS-PRO, el sistema debe autoprotegerse contra ataques de denegación de servicio (DDoS), fuerza bruta y scraping malicioso.

## 1. Rate Limiting Obligatorio
Ningún endpoint puede estar sin protección si es de alto riesgo:
- **Login / Auth:** Máximo 5 intentos por minuto (Previene fuerza bruta).
- **Exportaciones (PDF/Excel) o Tareas Pesadas:** Máximo 3 por minuto por usuario para evitar saturación de CPU/RAM.
- **APIs Públicas:** Límite estándar por IP/Token para evitar Data Scraping masivo.

## 2. Prevención de Costes Ocultos (Cloud Billing Attacks)
En tareas que llamen a APIs de terceros de pago (OpenAI, AWS, Pasarelas de pago, SMS), obligatoriamente se debe implementar un Rate Limiter estricto para evitar ataques que generen facturas astronómicas en la nube.
