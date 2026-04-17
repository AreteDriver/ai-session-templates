# Example Master Template Session

A filled-in master session template showing how to structure a multi-step task: adding a bulk operations endpoint group to an existing anomaly detection API. Demonstrates how the master template organizes work that spans route creation, service layer, database migration, tests, and documentation.

```md
# Session Context

## Objective
Add bulk operations endpoints to the Monolith anomaly detection API. Operators need to acknowledge, resolve, or suppress multiple anomalies in a single request instead of making individual PATCH calls per anomaly. This is the most-requested feature from the Discord bot integration.

## Repo / Project
- Name: monolith
- Purpose: EVE Frontier anomaly detection, enrichment, and alerting service
- Primary language(s): Python 3.12
- Stack: FastAPI, SQLAlchemy 2.0, SQLite (WAL), Alembic, pytest
- Environment: Fly.io (production), local dev with `uvicorn --reload`

## Current Task
Implement three bulk endpoints under `/api/v1/anomalies/bulk/`:
- `POST /bulk/acknowledge` -- mark multiple anomalies as acknowledged (sets `acked_at`, `acked_by`)
- `POST /bulk/resolve` -- resolve multiple anomalies (sets `resolved_at`, `resolution_note`)
- `POST /bulk/suppress` -- suppress anomalies by rule ID (prevents future alerts for matching rule)

Each endpoint accepts a JSON body with an array of anomaly IDs (max 100) and the operation-specific fields. Returns a result summary with per-item success/failure.

## Why This Task Matters
The Discord bot currently sends individual PATCH requests per anomaly. During a detection spike (e.g., a mass fuel burn event fires 50+ anomalies), the bot hammers the API with sequential requests, taking 15-20 seconds to acknowledge a batch. Operators lose trust in the tool when it takes that long. Bulk endpoints reduce this to a single request under 500ms.

## Scope
### In scope
- Three bulk endpoints with request/response schemas
- Service layer method for batch updates with per-item error handling
- Input validation (max 100 IDs per request, all IDs must exist, caller must have `write` scope)
- Partial success handling (some IDs succeed, some fail -- return both)
- Tests covering happy path, partial failure, empty array, over-limit, and auth

### Out of scope
- Bulk delete (destructive, needs separate design review)
- WebSocket push notification of bulk state changes (future work)
- UI changes in the dashboard (separate PR)
- Pagination of bulk results (100-item limit makes this unnecessary for now)

## Constraints
- Do not break existing behavior unless explicitly required
- Follow existing project conventions
- Prefer small, reviewable changes
- Route ordering: `/bulk/acknowledge` must register BEFORE `/{anomaly_id}` or FastAPI will capture "bulk" as an anomaly ID path parameter
- Bulk operations must be atomic per-item, not per-batch (one bad ID should not roll back the others)
- SQLite write performance: batch UPDATE with IN clause, not individual UPDATE per ID
- Scoped API key: all bulk endpoints require `write` or `admin` scope

## Codebase Guidance
- Router pattern: `backend/api/routes/anomalies.py` uses `APIRouter(prefix="/api/v1/anomalies")`
- Service pattern: `backend/services/anomaly_service.py` has `get_anomaly()`, `update_anomaly()`, `list_anomalies()`
- Schema pattern: `backend/schemas/anomaly.py` uses Pydantic v2 `BaseModel` with `model_config = ConfigDict(from_attributes=True)`
- Auth pattern: `backend/api/dependencies.py` exports `require_scope("write")` as a FastAPI dependency
- Error pattern: `backend/api/routes/anomalies.py` raises `HTTPException(404)` for missing entities

## Files / Areas Most Likely Relevant
- `backend/api/routes/anomalies.py` -- add bulk routes (BEFORE the `/{anomaly_id}` route)
- `backend/schemas/anomaly.py` -- add `BulkAcknowledgeRequest`, `BulkResolveRequest`, `BulkSuppressRequest`, `BulkOperationResult`
- `backend/services/anomaly_service.py` -- add `bulk_acknowledge()`, `bulk_resolve()`, `bulk_suppress()`
- `backend/api/dependencies.py` -- no changes expected (reuse existing `require_scope`)
- `tests/api/test_anomalies.py` -- add bulk operation test cases
- `tests/services/test_anomaly_service.py` -- add service-layer bulk tests

## Validation Requirements
- `pytest tests/api/test_anomalies.py -v` -- all existing + new tests pass
- `pytest tests/services/test_anomaly_service.py -v` -- service layer tests pass
- `pytest tests/ -v --tb=short` -- full suite passes, no regressions
- `ruff check . && ruff format --check .` -- zero lint issues
- Manual test: create 5 anomalies, bulk-acknowledge 3, verify 3 show `acked_at` and 2 do not
- Manual test: bulk-acknowledge with one invalid ID -- verify partial success response
- Manual test: send 101 IDs -- verify 422 validation error
- Auth test: send bulk request with `read`-only key -- verify 403

## Output Required
- Code changes implementing all three bulk endpoints
- Pydantic schemas for request/response
- Service layer with batch SQLite operations
- Test additions covering the cases listed above

## Deliverable Format
Return:
1. Summary of what was built and the design decisions made
2. Files changed with one-line description per file
3. Route registration order confirmation (bulk before parameterized)
4. Test results (pass count, any skips, coverage delta)
5. Performance note: expected latency for 100-item bulk operation on SQLite

## Avoid
- Generic event bus or command pattern -- just add methods to the existing service
- Async batch processing with background tasks -- these are fast synchronous writes
- Changing the existing single-item PATCH endpoint behavior
- Adding new dependencies for batch processing

## Extra Context
The Discord bot (`fleet-monitor`) calls these endpoints via `httpx`. The bot's current retry logic retries individual failures -- with bulk endpoints, it needs to parse the partial success response and retry only the failed IDs. The bot code is in a separate repo and will be updated in a follow-up PR. For now, the API contract is what matters.
```
