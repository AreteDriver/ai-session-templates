# Example Feature Session

A filled-in feature build template for adding webhook subscription endpoints to a FastAPI anomaly detection service backed by SQLite.

```md
# Feature Build Session

## Feature
Webhook subscription endpoints for the Monolith anomaly detection service.

## User / Operator Need
Downstream consumers (Discord bots, dashboards, alert aggregators) need real-time notification when new anomalies are detected. Currently they must poll `GET /api/v1/anomalies` on an interval, which wastes resources and introduces detection latency of up to 60 seconds. Webhook push delivery eliminates both problems.

## Desired Outcome
A set of CRUD endpoints under `/api/v1/webhooks` that let consumers register callback URLs, receive HMAC-signed POST payloads when anomalies are created or resolved, and manage their subscriptions. Delivery should be fire-and-forget with basic retry logic.

## Functional Requirements
- `POST /api/v1/webhooks` — register a subscription (url, event types, optional secret for HMAC)
- `GET /api/v1/webhooks` — list active subscriptions (paginated)
- `GET /api/v1/webhooks/{webhook_id}` — get subscription details + recent delivery status
- `DELETE /api/v1/webhooks/{webhook_id}` — remove a subscription
- `POST /api/v1/webhooks/{webhook_id}/test` — send a test payload to verify the endpoint
- Payload includes: `event_type`, `anomaly_id`, `rule_id`, `severity`, `detected_at`, `context_json`
- HMAC-SHA256 signature in `X-Signature-256` header when subscriber provides a secret
- Retry failed deliveries up to 3 times with exponential backoff (1s, 5s, 25s)
- Auto-disable subscriptions after 10 consecutive failures

## Non-Functional Requirements
- performance: webhook delivery must not block the anomaly detection pipeline. Use background task or queue
- security: validate callback URLs (no private IPs, no localhost). HMAC signing mandatory if secret provided
- maintainability: follow existing router/service/model pattern in the codebase
- compatibility: no changes to existing anomaly endpoints or detection logic

## Existing Patterns to Follow
- Router pattern: `backend/api/routes/anomalies.py` — standard FastAPI router with dependency injection
- Service layer: `backend/services/anomaly_service.py` — business logic separated from routes
- SQLite models: `backend/models/` — SQLAlchemy 2.0 declarative models, `sqlalchemy.Uuid` for PKs
- Background tasks: `backend/services/pruning_service.py` — uses FastAPI `BackgroundTasks` for async work
- Auth: `backend/api/dependencies.py` — scoped API key validation (`read`, `write`, `admin`)
- Config: `backend/core/config.py` — pydantic-settings with env var loading

## Files Likely Relevant
- `backend/api/routes/` — add `webhooks.py` router
- `backend/services/` — add `webhook_service.py` for delivery logic
- `backend/models/` — add `webhook.py` model (subscriptions table, delivery log table)
- `backend/api/dependencies.py` — webhook endpoints require `write` or `admin` scope
- `backend/services/anomaly_service.py` — add webhook dispatch call after anomaly creation
- `backend/core/config.py` — add `WEBHOOK_TIMEOUT_SECONDS`, `WEBHOOK_MAX_RETRIES` settings
- `alembic/versions/` — migration for new tables
- `tests/api/` — add `test_webhooks.py`

## Constraints
- Do not restructure existing anomaly detection pipeline
- Follow existing SQLAlchemy model conventions (Uuid PKs, created_at/updated_at timestamps)
- Webhook delivery failures must never crash or slow the detection loop
- Keep the diff reviewable — no speculative abstractions or generic event bus
- SQLite-compatible — no Postgres-specific features

## Validation
- Unit tests for webhook CRUD operations
- Unit tests for HMAC signature generation and verification
- Integration test for end-to-end flow: create subscription, trigger anomaly, verify payload received
- Test auto-disable after consecutive failures (mock httpx to return 500s)
- Test private IP rejection in URL validation
- Run full existing test suite to confirm no regressions: `pytest backend/tests/ -v --tb=short`
- Manual test: register a webhook pointing to `https://webhook.site`, trigger a detection rule, confirm payload arrives with correct signature

## Required Output
1. Implementation summary — what was built and why each piece exists
2. Files changed — full list with brief description of each change
3. Migration — the Alembic migration for new tables
4. Validation performed — which tests pass, what was manually verified
5. Risks and tradeoffs — anything deferred, any edge cases not covered
```
