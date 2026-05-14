# Infrastructure Reference

## Propósito

Este documento define criterios de infraestructura para proyectos NEXUS-4EVER. No obliga a usar una plataforma específica, pero sí exige que la app sea desplegable, monitoreable, recuperable y escalable.

## Componentes mínimos

- App Laravel.
- Frontend build.
- PostgreSQL.
- Redis.
- Queue workers.
- Scheduler/cron.
- Storage privado/público.
- Reverse proxy/Nginx.
- TLS.
- Backups.
- Health checks.
- Logs.

## Dockerfile

Debe:

- usar imagen base mantenida;
- instalar dependencias mínimas;
- no incluir secretos;
- usar usuario no root cuando sea posible;
- cachear capas correctamente;
- separar build de runtime si aplica.

## docker-compose local

Debe incluir:

- app;
- postgres;
- redis;
- mailpit;
- node/frontend si aplica;
- volúmenes persistentes;
- health checks básicos.

## Nginx

Debe configurar:

- TLS en producción;
- límites de upload;
- compresión;
- headers seguros;
- proxy timeouts;
- cache de assets;
- protección de archivos ocultos.

## Supervisor/workers

Workers deben:

- reiniciarse automáticamente;
- tener límites de memoria;
- tener timeouts;
- procesar colas específicas;
- loggear fallos;
- ser monitoreados.

## Scheduler

Laravel scheduler debe ejecutarse de forma única cuando hay múltiples nodos. Usar locks si aplica.

## Infrastructure as Code

Cuando el proyecto crezca, considerar:

- Terraform;
- Ansible;
- Helm/Kubernetes;
- scripts versionados;
- documentación de infraestructura.

## Checklist

- [ ] TLS habilitado.
- [ ] Health checks.
- [ ] Workers configurados.
- [ ] Scheduler configurado.
- [ ] Redis disponible.
- [ ] Backups configurados.
- [ ] Storage privado seguro.
- [ ] Logs centralizados.
- [ ] Rollback posible.
- [ ] Infra documentada.
