# Example Deploy Session

A filled-in deployment session template for deploying a FastAPI + SQLite service to Fly.io with Litestream backup to Tigris S3, covering Dockerfile review, fly.toml configuration, secrets, migration strategy, and rollback procedure.

```md
# Deploy Session

## Objective
Deploy the Overwatch ISR API to Fly.io with persistent SQLite storage and Litestream continuous backup to Tigris S3. This is the first production deploy -- no existing infrastructure to migrate from.

## Service Overview
- Name: overwatch-isr
- Stack: Python 3.12, FastAPI, SQLAlchemy 2.0, SQLite (WAL mode)
- Tests: 296 passing, 94% coverage
- Database: single SQLite file at `/data/overwatch.db`
- Migrations: Alembic (6 migration files)

## Dockerfile Review
Multi-stage build, Litestream installed in final image. Key concern: `COPY . .` picks up the full source tree -- confirm `.dockerignore` excludes `.git/`, `tests/`, `.env`, `__pycache__/`. Entrypoint is `scripts/run.sh` which handles restore, migration, and app startup.

## fly.toml Configuration
```toml
app = "overwatch-isr"
primary_region = "iad"
kill_signal = "SIGTERM"
kill_timeout = 30

[build]
  dockerfile = "Dockerfile"

[env]
  DATABASE_URL = "/data/overwatch.db"
  ENVIRONMENT = "production"
  LOG_LEVEL = "info"

[http_service]
  internal_port = 8080
  force_https = true
  auto_stop_machines = "suspend"
  auto_start_machines = true
  min_machines_running = 1

  [http_service.concurrency]
    type = "connections"
    hard_limit = 50
    soft_limit = 25

[[http_service.checks]]
  grace_period = "30s"
  interval = "15s"
  method = "GET"
  path = "/health"
  timeout = "5s"

[mounts]
  source = "overwatch_data"
  destination = "/data"
```

Critical settings: `min_machines_running = 1` (no cold starts), `auto_stop_machines = "suspend"` (preserves memory), volume at `/data` for SQLite persistence, health check at `/health`.

## Litestream + Entrypoint
Litestream replicates `/data/overwatch.db` to Tigris S3 (`overwatch-backup` bucket), 10s sync interval, 168h retention. The `run.sh` entrypoint: (1) `litestream restore -if-replica-exists` for rehydration on fresh deploys, (2) `alembic upgrade head` against the restored DB, (3) `exec litestream replicate -exec "uvicorn ..."` to wrap both processes in one container.

## Secrets Setup
Required secrets (set via `flyctl secrets set`):
```bash
/home/arete/.fly/bin/flyctl secrets set \
  SECRET_KEY="$(python -c 'import secrets; print(secrets.token_urlsafe(32))')" \
  LITESTREAM_ACCESS_KEY_ID="tid_XXXXXXXXXXXXXXXXXXXX" \
  LITESTREAM_SECRET_ACCESS_KEY="tsec_XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" \
  CORS_ORIGINS='["https://overwatch-dashboard.fly.dev"]' \
  -a overwatch-isr
```

Verify no secrets are in `fly.toml` `[env]` section -- those are plaintext in the repo.

## Pre-Deploy Checklist
- [ ] `.dockerignore` excludes `.git/`, `tests/`, `.env`, `__pycache__/`
- [ ] `fly.toml` has no secrets in `[env]`
- [ ] Tigris bucket created: `flyctl storage create -a overwatch-isr -n overwatch-backup`
- [ ] Volume created: `flyctl volumes create overwatch_data -a overwatch-isr --region iad --size 1`
- [ ] All secrets set via `flyctl secrets set`
- [ ] `alembic upgrade head` runs cleanly on a fresh SQLite file locally
- [ ] `run.sh` is executable and tested locally with `bash scripts/run.sh`
- [ ] Health check endpoint `/health` returns `{"status": "healthy"}` when DB is accessible

## Deploy Command
```bash
/home/arete/.fly/bin/flyctl deploy \
  --dockerfile Dockerfile \
  -a overwatch-isr \
  --wait-timeout 600
```

The `--wait-timeout 600` gives Alembic migrations time to complete before Fly.io considers the deploy failed.

## Smoke Test Plan
After deploy, run in order:
1. Health check: `curl https://overwatch-isr.fly.dev/health` -- expect `{"status": "healthy"}`
2. API version: `curl https://overwatch-isr.fly.dev/api/v1/version` -- expect version string
3. Auth gate: `curl -I https://overwatch-isr.fly.dev/api/v1/tracks` -- expect 401 without API key
4. Authenticated read: `curl -H "X-API-Key: $API_KEY" https://overwatch-isr.fly.dev/api/v1/tracks` -- expect 200 with empty array
5. Write test: POST a test track, GET it back, DELETE it
6. Litestream verify: check Tigris bucket for WAL segments -- `flyctl ssh console -a overwatch-isr -C "ls -la /data/"`
7. Logs: `flyctl logs -a overwatch-isr` -- confirm no errors, Litestream replication active

## Rollback Procedure
If the deploy is broken:
1. Check recent releases: `flyctl releases -a overwatch-isr`
2. Redeploy previous image: `flyctl deploy --image-ref <previous-image-ref> -a overwatch-isr`
3. If database is corrupted, restore from Litestream: `flyctl ssh console -a overwatch-isr` then `litestream restore -o /data/overwatch.db /data/overwatch.db`
4. If the volume is the problem, create a new one and restore: the Litestream backup in Tigris is the source of truth

## Constraints
- Single-machine deployment only (SQLite does not support multi-writer)
- Volume is region-locked to `iad` -- cannot failover to another region without Litestream restore
- Do not use `release_command` for Alembic -- it runs in a separate ephemeral machine without the volume mount. Run migrations inside `run.sh` instead
- Keep the Dockerfile under 15 layers to minimize image size

## Validation
- Deploy succeeds: `flyctl status -a overwatch-isr` shows `running`
- All 7 smoke test steps pass
- Litestream backup visible in Tigris within 60 seconds of first write
- Dashboard (`overwatch-dashboard.fly.dev`) can connect to the API
- No errors in `flyctl logs` for 10 minutes post-deploy
- Run `hey -n 100 -c 5 https://overwatch-isr.fly.dev/health` -- p99 latency under 500ms
```
