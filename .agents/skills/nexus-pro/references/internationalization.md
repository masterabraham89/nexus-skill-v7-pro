# Internationalization Reference

## Propósito

Este documento define reglas de internacionalización y localización para NEXUS-PRO, especialmente para contexto Perú.

---

## UTC en base de datos

Regla obligatoria:

- Guardar fechas y horas en UTC en la base de datos.
- Convertir a zona local solo en presentación.
- No guardar horas locales ambiguas en columnas críticas.
- Registrar timezone cuando el evento dependa del contexto local.

---

## America/Lima en presentación

Para Perú:

- Mostrar fechas en zona `America/Lima`.
- Convertir desde UTC en backend o frontend de forma controlada.
- Evitar conversiones duplicadas.
- Documentar si una fecha representa momento absoluto o fecha local.

---

## Locale es-PE

Usar locale `es-PE` para:

- Fechas.
- Números.
- Moneda.
- Separadores decimales.
- Formatos visibles.

Ejemplo:

```ts
new Intl.NumberFormat('es-PE', {
  style: 'currency',
  currency: 'PEN',
}).format(1234.56);
```

---

## Moneda PEN

Reglas:

- Usar PEN como moneda base en Perú.
- Mostrar símbolo `S/` cuando corresponda.
- No usar floats para dinero en backend.
- Usar `DECIMAL(12,2)` en PostgreSQL.
- Validar montos mínimos y máximos.
- Redondear de forma explícita.

---

## DECIMAL(12,2)

Para montos:

```sql
amount DECIMAL(12,2) NOT NULL
```

Evitar:

- `float`.
- `double`.
- Cálculos monetarios imprecisos.

---

## Validación de RUC

RUC Perú:

- 11 dígitos.
- Debe ser numérico.
- Puede iniciar con 10, 15, 17 o 20 según caso.
- Validar dígito verificador si el flujo lo requiere.
- Normalizar quitando espacios y guiones.

Regla backend:

- Validar longitud.
- Validar formato.
- Validar unicidad por empresa si aplica.

---

## Validación de DNI

DNI Perú:

- 8 dígitos.
- Numérico.
- Normalizar quitando espacios.
- No aceptar letras.
- Validar obligatoriedad según tipo de persona.

---

## Validación de celular Perú

Celular Perú:

- 9 dígitos.
- Normalmente inicia con 9.
- Puede mostrarse como `+51 9XXXXXXXX`.
- Guardar normalizado.
- Mostrar formateado.

---

## Formato de fechas

Recomendado en UI:

- Fecha corta: `dd/MM/yyyy`.
- Fecha con hora: `dd/MM/yyyy HH:mm`.
- Fecha larga: `13 de mayo de 2026`.
- Siempre aclarar timezone en reportes sensibles.

---

## Formato de moneda

Recomendado:

```text
S/ 1,234.56
```

Reglas:

- Mantener dos decimales.
- No mezclar símbolo y código sin necesidad.
- En reportes contables, usar formato consistente.
- En payloads API, enviar número decimal como string si se requiere precisión estricta.

---

## Textos traducibles

Frontend:

- Evitar hardcodear textos si el proyecto será multiidioma.
- Centralizar labels.
- Separar mensajes de error.
- Preparar keys por módulo.

Backend:

- Usar archivos de idioma para validaciones.
- Retornar códigos de error cuando el frontend traduzca.

---

## Checklist i18n

- [ ] UTC en base de datos.
- [ ] America/Lima en presentación.
- [ ] Locale es-PE aplicado.
- [ ] PEN usado correctamente.
- [ ] DECIMAL(12,2) para dinero.
- [ ] RUC validado.
- [ ] DNI validado.
- [ ] Celular Perú validado.
- [ ] Fechas consistentes.
- [ ] Textos preparados para traducción si aplica.
