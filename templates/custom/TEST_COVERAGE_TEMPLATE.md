---
version: "1.0.0"
updated: "2026-04-17"
---

# Test Coverage Template

For targeted test coverage improvement sessions. Focuses effort on the highest-ROI files first
and avoids the trap of chasing 100% by testing trivial code. Produces meaningful tests that
catch real bugs, not tests that exist to inflate a number.

```md
# Test Coverage Session

## Project Context
- Repo: [name + path]
- Language: [Python / Node / Rust / Go / etc.]
- Test framework: [pytest / vitest / jest / cargo test / etc.]
- Coverage tool: [pytest-cov / c8 / istanbul / tarpaulin / etc.]
- CI enforcement: [coverage gate at X% / no gate / reporting only]

## Coverage State

### Current Numbers
- Overall coverage: [X%]
- Target coverage: [Y%]
- Gap: [Y - X = Z percentage points]

### Coverage Report Summary
Lowest-coverage files that matter (skip generated code, migrations, config):

| File | Lines | Covered | Coverage % | Complexity | Priority |
|------|-------|---------|-----------|------------|----------|
| [path] | [N] | [N] | [X%] | [high/med/low] | [1-5] |
| [path] | [N] | [N] | [X%] | [high/med/low] | [1-5] |
| [path] | [N] | [N] | [X%] | [high/med/low] | [1-5] |
| [path] | [N] | [N] | [X%] | [high/med/low] | [1-5] |
| [path] | [N] | [N] | [X%] | [high/med/low] | [1-5] |

### Coverage Command
```bash
# Command to generate coverage report — copy-paste ready
# Python: pytest --cov=src --cov-report=term-missing --cov-report=html
# Node: vitest --coverage
[your command here]
```

## Prioritization Strategy

### Highest ROI (do first)
1. **Large files with low coverage and pure logic** — most lines gained per test written
2. **Core business logic** — where bugs actually hurt (validators, calculators, transformers)
3. **Error handling paths** — untested except/catch blocks hide real bugs
4. **Recently changed files with no tests** — new code without coverage is a regression waiting to happen

### Medium ROI (do if time permits)
5. **Utility/helper functions** — easy to test, often reused
6. **Data access layer** — query builders, ORM wrappers (with test DB)
7. **API endpoint handlers** — integration-level coverage

### Low ROI (skip unless specifically needed)
8. **Config/settings files** — tested by application startup
9. **Type definitions, dataclasses, models** — structural, not behavioral
10. **Generated code** — migrations, protobuf stubs, OpenAPI clients

## Test Patterns to Follow

### Existing Conventions
- Test file location: [tests/ mirror of src/ / colocated / etc.]
- Naming: [test_*.py / *.test.ts / *_test.go]
- Fixtures: [conftest.py / setup files — list key fixtures]
- Database setup: [in-memory SQLite / test container / mock / none]
- Factory pattern: [factory_boy / faker / custom builders / none]

### Test Structure
Follow existing test style in the repo. General guidelines:
- Arrange-Act-Assert (AAA) pattern
- One assertion per test when practical (exceptions: validating a complex return object)
- Descriptive test names: `test_[function]_[scenario]_[expected_result]`
- Group by module, not by test type

### What Makes a Good Coverage Test
- Tests a real code path, not just that a function exists
- Covers at least one branch (if/else, try/except, match/case)
- Uses realistic input data (not just empty strings and zeros)
- Asserts on behavior, not implementation details
- Fails if the code under test is deleted (not a tautology)

## Files to Skip
Do not write tests for these unless explicitly requested:

- [ ] Network clients (HTTP calls, WebSocket connections) — mock at boundary
- [ ] Background loops and schedulers (cron, polling) — test the handler, not the loop
- [ ] Third-party library wrappers with no custom logic
- [ ] CLI argument parsing (tested by integration/E2E)
- [ ] Logging configuration
- [ ] Migration files

## Test Plan

### Priority 1 (must cover this session)
| File | Target Functions/Methods | Test Type | Estimated Tests |
|------|------------------------|-----------|----------------|
| [path] | [function_a, function_b] | [unit] | [N] |
| [path] | [class.method_a] | [unit] | [N] |

### Priority 2 (cover if time permits)
| File | Target Functions/Methods | Test Type | Estimated Tests |
|------|------------------------|-----------|----------------|
| [path] | [function_c] | [unit] | [N] |
| [path] | [endpoint_handler] | [integration] | [N] |

## Constraints
- Tests must be deterministic — no flaky tests, no time-dependent assertions without freezing time
- No mocking external APIs unless the alternative is hitting a real service in CI
- Do not mock the thing you are testing (mock dependencies, not the subject)
- Prefer real objects over mocks when practical (in-memory DB over mocked queries)
- Do not write tests that test the framework (e.g., "FastAPI returns 404 for unknown routes")
- Do not artificially inflate coverage by testing getters/setters or config constants
- Every test must have a clear reason to exist — "what bug would this catch?"
- Match existing test style in the repo (do not introduce a new testing paradigm)

## Validation
- [ ] All new tests pass
- [ ] All existing tests still pass (no regressions)
- [ ] Coverage increased by [X] percentage points
- [ ] No flaky tests introduced (run suite 3x, all green)
- [ ] Coverage report regenerated and reviewed

## Required Output
1. Coverage before and after (overall % and per-file for targeted files)
2. Tests added (count, file locations)
3. Bugs found during testing (if any — this is a valuable side effect)
4. Files intentionally skipped and why
5. Remaining low-coverage files and recommended next session targets
6. Coverage command output (final run)
```
