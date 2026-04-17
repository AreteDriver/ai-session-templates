# Example Security Review Session

A filled-in security audit template for reviewing a Python web API before making the repository public and enabling external traffic.

```md
# Security Review Session

## Objective
Audit the Monolith anomaly detection API before switching the GitHub repo from private to public and opening the rate-limited public API (`/api/v1/`) to external consumers. The goal is to find and fix anything that would be embarrassing, exploitable, or leaky in a public-facing context.

## Repo
- Name: monolith
- Stack: Python 3.12, FastAPI, SQLAlchemy 2.0, SQLite (WAL mode), Fly.io
- Tests: 637 passing, 88% coverage
- Current access: private repo, internal-only API traffic

## Audit Scope

### 1. Dependency Scan
Run `pip-audit` against `requirements.txt` and report all findings with severity.

Expected output format:
```
Name        Version  ID                   Fix
----------  -------  -------------------  -------
pydantic    2.5.2    PYSEC-2026-XXX       2.5.3
cryptography 41.0.7  CVE-2026-XXXXX       42.0.0
```

Also check: are any dependencies pinned to versions with known CVEs in the `pyproject.toml` lock?

### 2. Secrets Scan
Run `gitleaks detect --source . --verbose` against the full git history.

Check for:
- API keys or tokens committed in any historical commit (even if later removed)
- `.env` files tracked in git (should be in `.gitignore`)
- Hardcoded secrets in test fixtures (e.g., `test_api_key = "real-key-here"`)
- Secrets in CI workflow files (should use `${{ secrets.X }}`)
- Fly.io deploy tokens in scripts

### 3. CORS Configuration
Review `backend/core/config.py` and `backend/main.py` for CORS middleware setup.

Check:
- Is `allow_origins` set to `["*"]`? (Must be restricted before public launch)
- Is `allow_credentials=True` combined with wildcard origins? (Browser security violation)
- Are allowed methods and headers appropriately scoped?
- Is `SecurityHeadersMiddleware` present and ordered BEFORE CORSMiddleware? (Starlette LIFO)

### 4. Rate Limiting
Review rate limiting implementation in `backend/api/middleware/` or route dependencies.

Check:
- Is rate limiting applied to all public endpoints?
- Is it per-IP or per-API-key? (Per-IP is bypassable behind proxies without `X-Forwarded-For` trust)
- What are the current limits? (Target: 60 req/min for unauthenticated, 300 req/min for keyed)
- Is there a rate limit on authentication endpoints? (Brute force prevention)
- Are rate limit headers returned? (`X-RateLimit-Limit`, `X-RateLimit-Remaining`, `Retry-After`)

### 5. Authentication and Authorization
Review `backend/api/dependencies.py` and route decorators.

Check:
- Are API keys validated via constant-time comparison? (`hmac.compare_digest`, not `==`)
- Are scopes enforced at the route level? (`read`, `write`, `admin`)
- Can a `read`-scoped key access `write` endpoints?
- Is the key hash stored in the database, not the raw key?
- Are admin endpoints (`/webhooks`, `/config`) properly gated?

### 6. Error Masking
Review exception handlers in `backend/main.py` and `backend/api/routes/`.

Check:
- Do 500 responses expose stack traces to the client? (Must return generic error, log details server-side)
- Do validation errors expose internal model field names? (Pydantic error responses can leak schema)
- Are database errors caught and masked? (`sqlalchemy.exc.OperationalError` should not reach the client)
- Is `debug=True` or `--reload` enabled in production config?

### 7. SQL Injection
Review all database query construction in `backend/services/`.

Check:
- Are all queries using parameterized statements? (`select().where(Model.field == value)`)
- Are there any raw SQL strings with f-string interpolation? (`text(f"SELECT * FROM {table}")`)
- Is user input ever passed directly to `.order_by()` or `.filter()` without validation?
- Check the 2 raw SQL calls in `backend/db/migrations.py`

### 8. SSRF Prevention
Review webhook delivery in `backend/services/webhook_service.py`.

Check:
- Are callback URLs validated against private IP ranges? (127.0.0.0/8, 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16, 169.254.169.254)
- Is DNS resolution validated? (A domain could resolve to a private IP)
- Is there a timeout on outbound webhook requests? (Default httpx timeout is no timeout)
- Are redirects followed? (Attacker can redirect from public to private IP)

## Severity Ratings
- **CRITICAL**: Exploitable by external attacker, immediate fix required before going public
- **HIGH**: Data leakage or privilege escalation possible, fix before launch
- **MEDIUM**: Defense-in-depth gap, fix within first sprint after launch
- **LOW**: Best practice violation, track as tech debt

## Constraints
- Do not run actual exploits against the production deployment
- Do not modify any production data or configuration during the review
- Fixes should be minimal and targeted -- no speculative refactoring
- Every finding must include: what, where (file:line), severity, and recommended fix
- Findings with no fix recommendation are not actionable and should be excluded

## Required Output
1. Findings table: one row per issue, columns: ID, Severity, Category, File, Description, Fix
2. Summary counts by severity (CRITICAL: N, HIGH: N, MEDIUM: N, LOW: N)
3. Blockers list: issues that must be fixed before the repo goes public
4. Recommended fix order: prioritized by severity and effort
5. Post-launch monitoring recommendations (what to watch for in the first week)

## Validation After Fixes
- Re-run `pip-audit` -- zero findings above LOW
- Re-run `gitleaks detect` -- zero findings
- Confirm CORS rejects requests from unlisted origins: `curl -H "Origin: https://evil.com" -v`
- Confirm rate limiter triggers: `hey -n 100 -c 20 https://monolith.fly.dev/api/v1/anomalies`
- Confirm error masking: trigger a 500 via malformed request, verify no stack trace in response body
- Full test suite: `pytest backend/tests/ -v --tb=short` -- 637 tests pass, no regressions
```
