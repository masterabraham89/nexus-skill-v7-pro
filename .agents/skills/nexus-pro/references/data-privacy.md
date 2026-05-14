# Data Privacy Reference

## Propósito

Este documento define reglas de privacidad y protección de datos para NEXUS-PRO, considerando Ley 29733 de Perú y GDPR como referencia de buenas prácticas.

---

## Principios base

- Legalidad.
- Consentimiento.
- Finalidad.
- Proporcionalidad.
- Calidad de datos.
- Seguridad.
- Disposición de recurso.
- Nivel de protección adecuado.

---

## Ley 29733 Perú

La Ley de Protección de Datos Personales exige tratar datos personales de manera lícita, informada, proporcional y segura.

Antigravity debe aplicar:

- Consentimiento cuando corresponda.
- Finalidad explícita.
- No recolectar datos innecesarios.
- Proteger datos personales.
- Permitir ejercicio de derechos.
- Controlar transferencias.
- Documentar incidentes.
- Aplicar medidas técnicas y organizativas.

---

## GDPR como referencia

Aunque el proyecto opere en Perú, GDPR sirve como estándar de referencia para:

- Privacy by design.
- Privacy by default.
- Minimización.
- Transparencia.
- Derechos del titular.
- Registro de tratamiento.
- Respuesta a brechas.
- Evaluación de impacto.

---

## Derechos ARCO

El sistema debe permitir o facilitar:

- Acceso.
- Rectificación.
- Cancelación.
- Oposición.

Considerar además:

- Portabilidad si aplica.
- Supresión si aplica.
- Limitación de tratamiento si aplica.

---

## Consentimiento

El consentimiento debe ser:

- Previo cuando aplique.
- Informado.
- Expreso cuando se requiera.
- Registrable.
- Revocable.

Registrar:

- Fecha.
- Versión de política.
- Usuario.
- Canal.
- Finalidad.
- IP/user agent si aplica.

---

## Minimización de datos

Regla:

Recolectar solo lo necesario.

Antes de agregar un campo, preguntar:

- ¿Es necesario para el negocio?
- ¿Tiene finalidad clara?
- ¿Se puede evitar?
- ¿Se puede anonimizar?
- ¿Cuánto tiempo debe conservarse?
- ¿Quién puede verlo?

---

## Anonimización

Usar anonimización para:

- Analytics.
- Reportes.
- Ambientes de desarrollo.
- Datasets de prueba.
- Métricas históricas.

Técnicas:

- Eliminación de identificadores.
- Hash irreversible.
- Generalización.
- Agrupación.
- Enmascaramiento.
- Tokenización según necesidad.

---

## Retención

Definir retención por dato:

- Usuarios.
- Clientes.
- Documentos.
- Logs.
- Auditoría.
- Backups.
- Sesiones.
- Tokens.
- Archivos.
- Reportes.

No mantener datos sin finalidad.

---

## Breach response

Ante brecha:

1. Contener.
2. Evaluar impacto.
3. Identificar datos afectados.
4. Preservar evidencia.
5. Rotar secretos si aplica.
6. Notificar internamente.
7. Evaluar notificación a autoridad/titulares.
8. Corregir causa raíz.
9. Documentar.
10. Prevenir recurrencia.

---

## Privacy by design

Desde diseño:

- Datos mínimos.
- Permisos mínimos.
- Cifrado.
- Auditoría.
- Separación por empresa.
- Retención.
- Logs seguros.
- Consentimiento.
- Exportación/eliminación cuando aplique.

---

## No usar datos de producción en desarrollo

Regla obligatoria:

- No copiar producción a desarrollo sin anonimización.
- No compartir dumps por canales inseguros.
- No usar datos reales para demos.
- No descargar bases completas innecesariamente.
- No exponer backups.
- No incluir PII en issues, logs o capturas.

---

## Seguridad de datos

Aplicar:

- Cifrado en tránsito.
- Cifrado en reposo si infraestructura lo permite.
- Control de acceso.
- Auditoría.
- Backups cifrados.
- Mínimos privilegios.
- Rotación de credenciales.
- Eliminación segura.

---

## Checklist privacy

- [ ] Finalidad clara.
- [ ] Datos mínimos.
- [ ] Consentimiento considerado.
- [ ] Derechos ARCO considerados.
- [ ] Retención definida.
- [ ] Logs sin PII innecesaria.
- [ ] Datos sensibles protegidos.
- [ ] No se usan datos de producción en desarrollo.
- [ ] Breach response considerado.
- [ ] Privacy by design aplicado.
