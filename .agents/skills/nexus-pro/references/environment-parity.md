# Environment Parity Reference

## Propósito

Este documento define reglas para mantener consistencia entre local, staging y producción. Diferencias grandes entre ambientes generan bugs invisibles, despliegues fallidos y errores difíciles de reproducir.

## Regla principal

Local, staging y producción deben usar versiones y configuración lo más parecidas posible, con datos seguros y variables explícitas.

## Versiones fijas

Documentar y fijar:

- PHP.
- Node.
- PostgreSQL.
- Redis.
- Composer.
- npm/pnpm/yarn.
- Extensiones PHP.
- Sistema operativo base si aplica.

## .env.example

Debe estar completo y sin secretos reales.

Incluir:

- APP_ENV.
- APP_URL.
- DB_*.
- REDIS_*.
- QUEUE_CONNECTION.
- MAIL_*.
- SANCTUM_*.
- STORAGE_*.
- THIRD_PARTY_* placeholders.

## Datos

- Local debe usar seeders fake.
- Staging debe usar datos sintéticos o anonimizados.
- Nunca usar datos de producción en desarrollo sin anonimización.
- Dumps deben estar cifrados y controlados.

## Docker

Recomendado:

- Dockerfile para app.
- docker-compose para local.
- Servicios: app, postgres, redis, mailpit.
- Scripts de setup.

## Config drift

Evitar diferencias no documentadas:

- collation DB;
- timezone;
- extensiones;
- workers;
- storage;
- cache;
- queue driver;
- limits upload;
- cron/scheduler.

## Checklist

- [ ] Versiones documentadas.
- [ ] .env.example completo.
- [ ] Sin secretos reales.
- [ ] Seeders seguros.
- [ ] Datos productivos no usados.
- [ ] Docker considerado.
- [ ] Staging similar a producción.
- [ ] Config drift revisado.
