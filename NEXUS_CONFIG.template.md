# NEXUS_CONFIG.md
# Project Configuration for NEXUS-PRO Skill
#
# Instructions:
#   1. Copy this file to the ROOT of your project (same level as composer.json / package.json)
#   2. Fill in the values that match your stack
#   3. Antigravity will read this file automatically when NEXUS-PRO is active
#   4. Leave unknown fields as-is — the skill will use safe defaults

# ─────────────────────────────────────────────
# PROJECT IDENTITY
# ─────────────────────────────────────────────
project:
  name: "My Enterprise App"
  type: "saas"                  # saas | pos | ecommerce | admin-panel | api-only | pwa | other
  language: "en"                # en | es | pt | fr  (language for code comments and logs)
  multi_tenant: true            # true | false (does the app serve multiple companies/orgs?)
  tenant_key: "company_id"      # field name used for tenant isolation (e.g. company_id, org_id, tenant_id)

# ─────────────────────────────────────────────
# BACKEND
# ─────────────────────────────────────────────
backend:
  framework: "laravel"          # laravel | nestjs | fastapi | express | django | none
  version: "11"                 # framework major version
  language: "php"               # php | typescript | python | javascript
  php_version: "8.3"            # only for Laravel projects

  auth:
    driver: "sanctum"           # sanctum | jwt | passport | auth0 | firebase | none
    token_header: "Authorization"

  database:
    primary: "postgresql"       # postgresql | mysql | sqlite | mongodb | none
    version: "16"
    orm: "eloquent"             # eloquent | prisma | drizzle | typeorm | sequelize | raw

  cache:
    driver: "redis"             # redis | memcached | database | array | none

  queue:
    driver: "redis"             # redis | database | sqs | rabbitmq | none
    connection: "default"

  storage:
    driver: "s3"                # s3 | local | gcs | cloudinary | none
    public_disk: "public"

# ─────────────────────────────────────────────
# FRONTEND
# ─────────────────────────────────────────────
frontend:
  framework: "react"            # react | vue | svelte | angular | none
  version: "18"
  language: "typescript"        # typescript | javascript
  bundler: "vite"               # vite | nextjs | webpack | remix | none

  state:
    server: "swr"               # swr | react-query | none
    client: "zustand"           # zustand | redux | jotai | context | none

  ui_library: "custom"          # shadcn | mui | antd | chakra | custom | none

  routing: "react-router-v6"    # react-router-v6 | tanstack-router | nextjs | none

  mobile:
    enabled: false              # true | false
    driver: "none"              # capacitor | react-native | pwa | none

# ─────────────────────────────────────────────
# TESTING
# ─────────────────────────────────────────────
testing:
  backend:
    framework: "pest"           # pest | phpunit | jest | vitest | pytest | none
    coverage_target: 80         # minimum % (0 to skip)

  frontend:
    framework: "vitest"         # vitest | jest | none
    e2e: "playwright"           # playwright | cypress | none

# ─────────────────────────────────────────────
# DEPLOYMENT
# ─────────────────────────────────────────────
deployment:
  target: "vps"                 # vercel | railway | aws | digitalocean | vps | fly | none
  ci_cd: "github-actions"       # github-actions | gitlab-ci | circleci | none
  container: false              # true | false (Docker/Kubernetes)
  environments:
    - staging
    - production

# ─────────────────────────────────────────────
# OBSERVABILITY
# ─────────────────────────────────────────────
observability:
  logging: "laravel"            # laravel | winston | pino | sentry | datadog | none
  monitoring: "none"            # sentry | datadog | newrelic | none
  apm: false                    # Application Performance Monitoring (true | false)

# ─────────────────────────────────────────────
# REAL-TIME
# ─────────────────────────────────────────────
realtime:
  enabled: false                # true | false
  driver: "none"                # pusher | soketi | ably | websocket | sse | none
  channel_prefix: ""            # e.g. "orders-", "tenant-"

# ─────────────────────────────────────────────
# ADVANCED FEATURES
# ─────────────────────────────────────────────
features:
  offline_sync: false           # IndexedDB + sync queue (true | false)
  financial_audit: false        # strict financial traceability (true | false)
  file_uploads: false           # file upload module active (true | false)
  exports: false                # CSV/Excel/PDF exports (true | false)
  notifications: false          # in-app or push notifications (true | false)
  i18n: false                   # multi-language content (true | false)
  dark_mode: false              # dark mode support (true | false)
