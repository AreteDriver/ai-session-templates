# Example Bug Fix Session

A filled-in bugfix template for diagnosing SQLite WAL checkpoint deadlocks in a FastAPI service running on Fly.io with Litestream replication.

```md
# Bug Fix Session

## Problem
The license server (`cmdf-license` on Fly.io) intermittently returns 500 errors on key validation requests. Logs show `sqlite3.OperationalError: database is locked` during peak traffic windows (typically 10-30 second bursts). The issue started after enabling Litestream for continuous SQLite replication to Tigris S3.

## Expected Behavior
Key validation requests (`GET /v1/validate/{key}`) should return within 200ms consistently, even while Litestream is performing WAL checkpoints in the background.

## Actual Behavior
Under concurrent load (3+ validation requests overlapping with a Litestream checkpoint), SQLite's WAL checkpoint and application writes contend for the same lock. The default `busy_timeout` of 5000ms is not sufficient — Litestream holds the checkpoint lock longer than expected on Fly.io shared-cpu VMs, causing application queries to time out and raise `database is locked`.

Observed pattern in `flyctl logs`:
```
2026-04-10T03:14:22Z app[e784] ERROR: sqlite3.OperationalError: database is locked
2026-04-10T03:14:22Z app[e784] File "license_server/db.py", line 47, in validate_key
2026-04-10T03:14:22Z app[e784]   cursor.execute("SELECT * FROM licenses WHERE key_hash = ?", (key_hash,))
```

## Reproduction
1. Deploy `cmdf-license` to Fly.io with Litestream enabled (`litestream.yml` replicating to Tigris)
2. Run `hey -n 50 -c 10 https://cmdf-license.fly.dev/v1/validate/ANMD-TEST-XXXX-XXXX` to simulate concurrent validation
3. Observe 500 errors in ~10-20% of requests during Litestream checkpoint windows
4. Confirm locally: run `litestream replicate` alongside `uvicorn license_server.main:app` and hit the endpoint with concurrent requests

## Suspected Area
- `license_server/db.py` — SQLite connection setup, `busy_timeout` configuration
- `license_server/main.py` — FastAPI lifespan, connection pool (if any)
- `litestream.yml` — checkpoint interval and mode settings
- `license_server/Dockerfile` — Litestream entrypoint wrapper

## Severity
high — production-blocking for paying customers during peak validation windows. Fail-open client cache masks some failures, but uncached first-validation requests fail hard.

## Root Cause Hypothesis
Three factors combine:
1. SQLite `busy_timeout` is set to 5s, but Litestream WAL checkpoints on shared-cpu-1x VMs can hold the lock for 8-15s under I/O pressure
2. `check_same_thread=False` is set (required for FastAPI), but no connection pooling or retry logic exists
3. Litestream defaults to `PASSIVE` checkpoint mode, which is fine, but the Fly.io VM's disk I/O is slow enough that even passive checkpoints block longer than expected

## Constraints
- Fix root cause, not cosmetic symptom — do not just increase `busy_timeout` to 30s
- Preserve current API behavior — no schema changes, no endpoint signature changes
- Keep Litestream replication working — cannot disable it (backup is critical)
- Add or update tests to cover concurrent access patterns
- Minimize Dockerfile changes — avoid adding new system dependencies

## Required Output
1. Root cause — confirmed lock contention path between Litestream checkpoints and application writes
2. Fix — the actual code changes with rationale for each
3. Impacted files — list every file touched
4. Test coverage — how concurrent access is now validated
5. Residual risk — any remaining failure modes and their mitigations

## Likely Fix Direction
- Set `journal_mode=WAL` explicitly at connection time (not just rely on Litestream setting it)
- Increase `busy_timeout` to 15000ms as a safety net
- Add retry-with-backoff wrapper around `cursor.execute()` calls in `db.py`
- Configure Litestream checkpoint interval to avoid overlapping with high-traffic windows if possible
- Consider connection pooling via `aiosqlite` or a simple thread-local pattern

## Validation
- Reproduce the failure locally before applying any fix
- After fix, re-run the concurrent load test (`hey -n 50 -c 10`) and confirm zero 500 errors
- Run existing test suite: `pytest license_server/tests/ -v`
- Verify Litestream replication still works: check Tigris S3 bucket for fresh WAL segments after deploy
- Monitor `flyctl logs` for 24 hours post-deploy to confirm no recurrence
```
