# NEXUS_CONFIG.md — Example: NestJS SaaS with Vue 3 and MySQL
#
# This is an example of a project using a different stack than the default.
# It shows how NEXUS-PRO adapts to NestJS, Vue 3, MySQL, and JWT auth.

project:
  name: "BookingPro"
  type: "saas"
  language: "en"
  multi_tenant: true
  tenant_key: "org_id"          # This project uses org_id instead of company_id

backend:
  framework: "nestjs"           # NestJS instead of Laravel
  version: "10"
  language: "typescript"
  php_version: null             # Not a PHP project

  auth:
    driver: "jwt"               # JWT instead of Sanctum
    token_header: "Authorization"

  database:
    primary: "mysql"            # MySQL instead of PostgreSQL
    version: "8.0"
    orm: "typeorm"              # TypeORM instead of Eloquent

  cache:
    driver: "redis"

  queue:
    driver: "redis"

  storage:
    driver: "s3"
    public_disk: "public"

frontend:
  framework: "vue"              # Vue 3 instead of React
  version: "3"
  language: "typescript"
  bundler: "vite"

  state:
    server: "vue-query"         # Vue Query (TanStack Query for Vue)
    client: "pinia"             # Pinia instead of Zustand

  ui_library: "shadcn"          # shadcn-vue

  routing: "vue-router"

  mobile:
    enabled: true
    driver: "capacitor"         # Capacitor for mobile

testing:
  backend:
    framework: "jest"           # Jest for NestJS
    coverage_target: 75

  frontend:
    framework: "vitest"
    e2e: "playwright"

deployment:
  target: "railway"
  ci_cd: "github-actions"
  container: true               # Docker
  environments:
    - staging
    - production

observability:
  logging: "winston"            # Winston for NestJS
  monitoring: "sentry"
  apm: false

realtime:
  enabled: true
  driver: "pusher"
  channel_prefix: "booking-"

features:
  offline_sync: false
  financial_audit: true         # Booking history must be fully auditable
  file_uploads: true            # Profile photos, documents
  exports: true                 # Booking reports in CSV
  notifications: true           # Email + push notifications
  i18n: true                    # Spanish and English
  dark_mode: true
