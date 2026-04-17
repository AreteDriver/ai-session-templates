# Example Test Coverage Grind Session

A filled-in session template for a targeted test coverage push from 70% to 80% on a Python backend, showing coverage analysis, file-by-file prioritization, skip decisions, and validation workflow.

```md
# Test Coverage Session

## Objective
Push the witness backend test coverage from 70% to 80% (minimum). The repo has 774 tests passing. Coverage is concentrated in the core detection and enrichment modules (90%+) but thin in the API routes, Discord command handlers, and blockchain integration layers. The 80% threshold is the floor for adding a CI coverage gate.

## Current State
```
pytest tests/ --cov=backend --cov-report=term-missing -q

Name                                         Stmts   Miss  Cover
----------------------------------------------------------------
backend/api/routes/anomalies.py                142     18    87%
backend/api/routes/entities.py                 118     31    74%
backend/api/routes/health.py                    14      0   100%
backend/api/routes/intel.py                     96     29    70%
backend/api/routes/ssu.py                      134     52    61%
backend/api/routes/webhooks.py                 108     12    89%
backend/api/dependencies.py                     48      3    94%
backend/core/config.py                          32      0   100%
backend/db/session.py                           28      4    86%
backend/models/anomaly.py                       44      0   100%
backend/models/entity.py                        56      0   100%
backend/models/killmail.py                      38      0   100%
backend/services/anomaly_service.py            186      8    96%
backend/services/briefing_service.py            94     41    56%
backend/services/detection/rule_engine.py      224     11    95%
backend/services/detection/rules/*.py          412     20    95%
backend/services/enrichment_service.py         148     14    91%
backend/services/oracle_service.py             176     68    61%
backend/services/sui_client.py                 134     87    35%
backend/services/webhook_service.py             92      6    93%
backend/discord/commands/*.py                  286    158    45%
backend/discord/bot.py                          64     38    41%
----------------------------------------------------------------
TOTAL                                         2648    600    77% (misleading -- pytest-cov counts __init__.py)
```

Actual meaningful coverage: ~70% when weighted by complexity.

## Strategy: ROI-Based File Prioritization

### Tier 1: High ROI (large uncovered blocks, easy to test, no external deps)
These files have straightforward logic behind their uncovered lines. Existing test fixtures and factories cover the setup. Target: 85%+ each.

| File | Current | Gap (lines) | Effort | Notes |
|------|---------|-------------|--------|-------|
| `routes/entities.py` | 74% | 31 | Small | Missing: filter combos, pagination edge cases, 404 on bad ID |
| `routes/intel.py` | 70% | 29 | Small | Missing: date range validation, empty result sets, auth scope checks |
| `routes/ssu.py` | 61% | 52 | Medium | Missing: compact entity lookup, display_name defaulting, malformed input |
| `briefing_service.py` | 56% | 41 | Medium | Missing: LLM error handling, cache hit/miss, empty context |

### Tier 2: Medium ROI (requires mocking external services)
These need httpx mocks, Anthropic client mocks, or async event loop fixtures. Worth doing but slower to write.

| File | Current | Gap (lines) | Effort | Notes |
|------|---------|-------------|--------|-------|
| `oracle_service.py` | 61% | 68 | Medium | Mock WatchTower API responses, test subscription lifecycle |
| `routes/anomalies.py` | 87% | 18 | Small | Missing: bulk status edge cases, concurrent update handling |

### Tier 3: Low ROI (skip for now)
These have heavy external dependencies that make unit testing expensive relative to the coverage gain. Integration tests are more appropriate and belong in a separate effort.

| File | Current | Why Skip |
|------|---------|----------|
| `sui_client.py` | 35% | RPC calls to Sui testnet. Mock-heavy tests would test the mock, not the code. Better as integration tests with a local node |
| `discord/commands/*.py` | 45% | discord.py interaction mocking is brittle and breaks on library updates. Cover via E2E bot tests |
| `discord/bot.py` | 41% | Lifecycle management (connect, disconnect, error handlers). Hard to unit test meaningfully |

## Test Patterns to Follow
Existing conventions from `tests/api/test_anomalies.py` -- match this style for all new tests:
- Route tests: `TestClient` with `app.dependency_overrides` for DB session and auth scope injection
- Service tests: `@patch` with `AsyncMock` for httpx clients (`MagicMock` for sync response methods, `AsyncMock` only for `__aenter__`/`__aexit__`)
- Factory pattern: `make_anomaly(db_session, **overrides)` in `tests/factories.py` -- use `defaults.update(overrides)` pattern for all entity creation

## Execution Plan

### Phase 1: Tier 1 Files (target: +7% coverage)
1. `tests/api/test_entities.py` -- add 8 tests: filter by type, filter by system, pagination (page 0, page beyond range, negative page), 404 on nonexistent ID, empty search result, combined filters
2. `tests/api/test_intel.py` -- add 6 tests: date range validation (end before start), missing required params, empty results for valid date range, auth with read scope, auth denied with no scope
3. `tests/api/test_ssu.py` -- add 10 tests: compact entity lookup, display_name == entity_id[:12] default check, malformed entity ID (not UUID), missing entity, multiple entity resolve, truncated response format
4. `tests/services/test_briefing_service.py` -- add 8 tests: LLM returns empty string, LLM raises timeout, cache hit returns cached briefing, cache miss calls LLM, empty activity context, system with no kills, max_tokens respected, terse prompt format

Run after Phase 1: `pytest tests/ --cov=backend -q` -- expect ~77%

### Phase 2: Tier 2 Files (target: +4% coverage)
5. `tests/services/test_oracle_service.py` -- add 10 tests: subscription create, subscription already exists, WatchTower returns 403 (NEXUS expired), poll returns events, poll returns empty, connection timeout, retry on 500, parse malformed event, subscription auto-refresh
6. `tests/api/test_anomalies.py` -- add 4 tests: bulk status with mixed valid/invalid IDs, bulk status with empty list, concurrent acknowledge (optimistic lock), anomaly already resolved

Run after Phase 2: `pytest tests/ --cov=backend -q` -- expect ~81%

### Phase 3: Validation
7. Full suite run with verbose output: `pytest tests/ -v --tb=short --cov=backend --cov-report=term-missing`
8. Confirm no test failures introduced
9. Confirm coverage >= 80% on the `TOTAL` line
10. Run `ruff check . && ruff format --check .` -- ensure test files pass lint
11. Check for flaky tests: `pytest tests/ -v --count=3 -x` (with pytest-repeat if installed)
12. Verify no `@pytest.mark.skip` added as a shortcut

## Constraints
- Do not refactor production code to make it more testable. Test what exists.
- Do not add tests that only test mocks (if you mock everything, you tested nothing).
- Do not add `# pragma: no cover` to skip lines. The goal is real coverage.
- Do not change existing test behavior -- only add new test functions.
- All new tests must follow the existing fixture and factory patterns.
- Keep test names descriptive: `test_list_entities_filter_by_type_returns_filtered` not `test_1`.

## Required Output
1. Test files changed/created, with count of new test functions per file
2. Coverage report showing before and after percentages per file
3. Total coverage number (must be >= 80%)
4. Any files where coverage decreased (should be zero)
5. Any tests that are flaky or order-dependent (flag for follow-up)
6. Recommendation: what would it take to get to 85%? (brief, not a full plan)
```
