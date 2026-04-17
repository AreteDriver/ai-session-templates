# Migration Template

For database migrations, framework upgrades, API version bumps, and any change that moves a system
from one stable state to another. Forces explicit inventory of breaking changes, rollback planning,
and validation at every stage.

```md
# Migration Session

## Migration Type
[database schema / framework upgrade / API version bump / language version / dependency major version / infrastructure move]

## Project Context
- Repo: [name + path]
- Language: [Python / Node / Rust / Go / etc.]
- Framework: [FastAPI / Django / Express / Next.js / etc.]
- Database: [PostgreSQL / SQLite / MySQL / none]
- ORM/Migration tool: [Alembic / Django migrations / Prisma / Knex / raw SQL / none]
- Deployment: [Fly.io / Vercel / AWS / local only]

## Current State
- Version/schema: [what we have now]
- Last migration: [ID or description]
- Known technical debt affecting migration: [list or "none"]
- Active users/traffic: [yes/no, approximate scale]

## Target State
- Version/schema: [what we are moving to]
- Why: [security patch / deprecation deadline / new feature requires it / performance]
- Changelog/upgrade guide link: [URL if applicable]

## Breaking Changes Inventory

### Schema Changes (if database migration)
| Change | Table/Collection | Type | Data Impact |
|--------|-----------------|------|-------------|
| [description] | [table] | [add column / drop column / rename / type change / constraint] | [none / backfill needed / data loss risk] |

### API Changes (if API version bump)
| Endpoint | Change | Clients Affected |
|----------|--------|-----------------|
| [path] | [removed / renamed / request body changed / response shape changed] | [list consumers] |

### Dependency Changes (if framework/library upgrade)
| Package | From | To | Breaking Changes |
|---------|------|----|-----------------|
| [name] | [version] | [version] | [summary of what breaks] |

### Configuration Changes
- [env vars added/removed/renamed]
- [config file format changes]
- [feature flags needed for gradual rollout]

## Dependency Chain
Order matters. List what must migrate first.
1. [dependency or step — e.g., "upgrade ORM before running new migrations"]
2. [dependency or step]
3. [dependency or step]

## Data Preservation
- [ ] All existing data survives migration (no rows dropped, no columns silently removed)
- [ ] Default values set for new non-nullable columns
- [ ] Data backfill script written and tested (if needed)
- [ ] Large table migration strategy: [online DDL / batched updates / maintenance window]
- [ ] Backup taken before migration: [command or process]

## Rollback Plan
- Rollback method: [reverse migration / restore from backup / feature flag / redeploy previous version]
- Rollback command: [exact command or steps]
- Rollback time estimate: [minutes]
- Data written after migration: [recoverable / lost on rollback — document which]
- Point of no return: [describe the step after which rollback becomes significantly harder]

## Backward Compatibility Window
- Duration: [how long old and new must coexist — e.g., "2 weeks for API consumers to update"]
- Strategy: [version header routing / dual-write / feature flag / shadow mode]
- Deprecation notice: [how clients are informed — changelog, header warnings, logs]

## Validation

### Before Migration
- [ ] Backup verified (can restore to current state)
- [ ] Migration tested against copy of production data
- [ ] All tests pass on current version
- [ ] Dependency chain order confirmed
- [ ] Rollback procedure tested

### During Migration
- [ ] Migration script runs without errors
- [ ] No unexpected data loss (row counts, spot checks)
- [ ] Application starts successfully after migration
- [ ] Health endpoint responds

### After Migration
- [ ] All tests pass on new version
- [ ] Key user flows tested manually (list critical paths)
- [ ] Performance baseline compared (no regression)
- [ ] Monitoring confirms normal error rates
- [ ] Old version/schema artifacts cleaned up (or scheduled for cleanup)

## Constraints
- No data loss — every row that exists before must exist after
- Maintain backward compatibility for [duration] unless explicitly waived
- Migration must be reversible until [point of no return]
- Downtime budget: [zero / N minutes maintenance window / off-peak only]
- Do not combine migration with feature work in the same change

## Required Output
1. Migration plan (ordered steps with estimated time per step)
2. Migration scripts or commands (copy-paste ready)
3. Breaking changes summary (for downstream consumers)
4. Rollback procedure (tested, with exact commands)
5. Validation results (before/during/after checklists completed)
6. Residual risks (what could still go wrong, and mitigation)
```
