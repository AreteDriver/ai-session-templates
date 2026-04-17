# Example Migration Session

A filled-in session template for upgrading SQLAlchemy 1.4 to 2.0 in a FastAPI service with async sessions, covering breaking changes inventory, file-by-file migration, rollback plan, and test validation.

```md
# Migration Session

## Objective
Upgrade SQLAlchemy from 1.4.52 to 2.0.x in the Dossier API service. The 1.4 line is EOL (no security patches since January 2026). The upgrade unblocks adopting the new `DeclarativeBase` pattern and `Mapped[]` type annotations, which the rest of the fleet already uses.

## Current State
- SQLAlchemy 1.4.52 pinned in `pyproject.toml`
- 18 model files using `declarative_base()` from `sqlalchemy.orm`
- Service layer uses `session.query(Model).filter(...)` pattern throughout
- Async sessions via `create_async_engine` + `AsyncSession` (already present in 1.4 via extensions)
- 1,084 tests passing, 97% coverage
- Deployed on Fly.io with Litestream SQLite replication

## Breaking Changes Inventory
Audited against SQLAlchemy 2.0 migration guide. These are the actual breaks in this codebase:

### Critical (will crash at import or first query)
1. `declarative_base()` removed -- must switch to `class Base(DeclarativeBase)` in `backend/models/base.py`
2. `session.query(Model)` removed -- must rewrite to `session.execute(select(Model))` in 14 service files
3. `Query.get()` removed -- replace with `session.get(Model, id)` in 8 locations
4. `engine.execute()` removed -- 2 raw SQL calls in `backend/db/migrations.py` must use `with engine.connect() as conn: conn.execute(text(...))`

### High (will produce incorrect results or warnings-as-errors)
5. `Column(Integer)` still works but `Mapped[int]` is the new pattern -- convert for type checker support
6. `relationship()` backref string deprecated -- switch to `back_populates` (already done in 12/18 models, 6 remaining)
7. `sqlalchemy.Uuid` replaces `sqlalchemy.dialects.postgresql.UUID` -- already using portable `Uuid` so no change needed

### Low (deprecation warnings, non-blocking)
8. `Session.close()` behavior changed -- async sessions now require `await session.close()` (already correct in our code)
9. `autocommit` parameter removed from `Session()` -- we use `expire_on_commit=False` instead, no change needed

## File-by-File Migration Plan

### Phase 1: Base and Models (no behavior change)
- `backend/models/base.py` -- replace `Base = declarative_base()` with `class Base(DeclarativeBase): pass`
- `backend/models/entity.py` -- convert `Column()` to `Mapped[]` annotations, fix 2 backref strings
- `backend/models/briefing.py` -- same column conversion, no relationship changes
- `backend/models/intel_report.py` -- same pattern, plus fix `relationship(backref="reports")` to `back_populates`
- Remaining 14 model files -- mechanical conversion, same pattern

### Phase 2: Service Layer (query syntax)
- `backend/services/entity_service.py` -- 11 `session.query()` calls to `select()` + `session.execute()`
- `backend/services/briefing_service.py` -- 8 query rewrites
- `backend/services/intel_service.py` -- 6 query rewrites, including 2 `.get()` calls
- `backend/services/search_service.py` -- 4 query rewrites with joins (most complex)
- `backend/services/sync_service.py` -- 3 query rewrites

### Phase 3: Database Utilities
- `backend/db/migrations.py` -- 2 raw `engine.execute()` calls to `conn.execute(text(...))`
- `backend/db/session.py` -- update `create_async_engine` import path (moved in 2.0)
- `alembic/env.py` -- update `target_metadata` import to use new `Base.metadata`

### Phase 4: Test Fixtures
- `tests/conftest.py` -- update session fixture to use 2.0 `async_session_maker`
- `tests/factories.py` -- update any `session.add()` patterns if needed

## Query Rewrite Pattern
Before (1.4):
```python
users = session.query(User).filter(User.active == True).all()
user = session.query(User).get(user_id)
count = session.query(func.count(User.id)).scalar()
```

After (2.0):
```python
users = (await session.execute(select(User).where(User.active == True))).scalars().all()
user = await session.get(User, user_id)
count = (await session.execute(select(func.count(User.id)))).scalar_one()
```

## Rollback Plan
- Git branch: `feat/sqla-2.0-upgrade` off `main` at commit before any changes
- If tests fail after Phase 2 and fix is not obvious within 30 minutes, revert to branch point
- `pyproject.toml` pin makes rollback a single-line revert + `pip install -e .`
- Fly.io rollback: `flyctl releases -a dossier-api` then `flyctl deploy --image-ref <previous>`
- No database schema changes involved -- rollback has zero data risk

## Constraints
- Do not change any API endpoint signatures or response shapes
- Do not introduce new dependencies (no `sqlmodel`, no `databases`)
- Keep async session pattern -- do not regress to sync sessions
- All 1,084 tests must pass before merging
- Run `ruff check . && ruff format --check .` -- zero lint regressions
- Migration must be deployable in a single release (no phased rollout)

## Validation
1. After Phase 1 (models): `pytest tests/models/ -v` -- all model tests pass
2. After Phase 2 (services): `pytest tests/services/ -v` -- all service tests pass
3. After Phase 3 (db utils): `pytest tests/db/ -v` -- migration and session tests pass
4. Full suite: `pytest tests/ -v --tb=short` -- 1,084 tests pass, coverage stays above 95%
5. Import check: `python -c "from backend.models.base import Base; print(Base.__name__)"` -- no import errors
6. Deprecation scan: `python -W error::DeprecationWarning -m pytest tests/ -x` -- zero SQLAlchemy deprecation warnings
7. Lint: `ruff check . && ruff format --check .` -- clean
8. Local smoke test: `uvicorn backend.main:app` then hit `GET /api/entities` and `POST /api/ai/briefing`

## Required Output
1. Completed migration with all 4 phases applied
2. List of every file changed with a one-line description
3. Total query rewrites performed (count)
4. Test results summary (pass/fail/skip counts, coverage delta)
5. Any behavioral differences discovered during migration (even if tests pass)
6. Residual deprecation warnings, if any
```
