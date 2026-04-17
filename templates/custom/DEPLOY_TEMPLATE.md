# Deploy Template

For deployment sessions targeting Fly.io, Vercel, cloud providers, or any production push.
Structures the full deploy lifecycle: pre-flight validation, execution, smoke testing, and
rollback readiness. Prevents "deploy and pray" by making every step explicit.

```md
# Deploy Session

## Service
- Name: [service/app name]
- Repo: [name + path]
- Platform: [Fly.io / Vercel / AWS / GCP / bare metal]
- Environment: [production / staging / preview]
- URL: [production URL]
- Current version: [tag / commit / version]

## Target Changes
[What is being deployed and why. Link to PR or commit range.]

### Changes Included
- [change 1 — e.g., "new /api/v2/search endpoint"]
- [change 2 — e.g., "Alembic migration: add index on events.created_at"]
- [change 3]

### Changes Explicitly NOT Included
- [feature branch not ready — stays out of this deploy]

## Pre-Flight Checklist

### Code Quality
- [ ] All tests pass locally
- [ ] CI pipeline green on this commit
- [ ] No lint warnings or type errors
- [ ] No debug/development code left in (print statements, TODO hacks)

### Configuration
- [ ] Environment variables set on platform: [list new/changed vars]
- [ ] Secrets rotated if needed: [list or "none"]
- [ ] Config files updated: [list or "none"]
- [ ] Feature flags set correctly: [list or "none"]

### Database
- [ ] Migrations tested against production-like data
- [ ] Migration is backward-compatible (old code can run against new schema)
- [ ] Backup taken before migration: [command]
- [ ] Large table migration strategy: [online DDL / maintenance window / N/A]

### Dependencies
- [ ] No new dependencies with known CVEs
- [ ] Lock file committed (poetry.lock / package-lock.json / Cargo.lock)
- [ ] Native dependencies available on deploy platform

### Infrastructure
- [ ] Dockerfile builds cleanly (if containerized)
- [ ] Health endpoint responds: [GET /health or equivalent]
- [ ] Resource limits set: [memory, CPU, concurrency]
- [ ] Scaling config reviewed: [min/max instances, auto-scale triggers]

## Deploy Command
```bash
# Exact command to deploy — copy-paste ready
# Fly.io example:
# /home/arete/.fly/bin/flyctl deploy --wait-timeout 600

# Vercel example:
# vercel --prod

[your deploy command here]
```

## Deploy Sequence
1. [Take database backup]
2. [Run migrations if needed]
3. [Deploy application]
4. [Wait for health check to pass]
5. [Run smoke tests]
6. [Monitor error rates for 10 minutes]

## Smoke Test Plan
Run immediately after deploy confirms healthy.

| Test | Method | Expected Result |
|------|--------|----------------|
| Health endpoint | `curl https://[url]/health` | 200 OK, JSON with status |
| Auth flow | [manual or automated] | Login succeeds, token returned |
| Core read path | `curl https://[url]/api/[endpoint]` | 200 OK, valid response |
| Core write path | [POST to test endpoint] | 201 Created, data persisted |
| New feature | [specific test for this deploy's changes] | [expected behavior] |
| Error handling | [request with bad input] | Proper error response, no stack trace |

## Rollback Procedure

### Trigger Conditions
Roll back immediately if any of these occur:
- Health endpoint returns non-200 for > 2 minutes
- Error rate exceeds [X]% (normal baseline: [Y]%)
- Critical user flow is broken (auth, core read/write)
- Data corruption detected

### Rollback Steps
```bash
# Exact rollback command — copy-paste ready
# Fly.io example:
# /home/arete/.fly/bin/flyctl releases --app [app-name]
# /home/arete/.fly/bin/flyctl deploy --image [previous-image]

# Vercel example:
# vercel rollback

[your rollback command here]
```

### Post-Rollback
- [ ] Verify rollback succeeded (health check, smoke test)
- [ ] Reverse database migration if needed: [command or "N/A — migration was backward-compatible"]
- [ ] Notify stakeholders: [channel or "N/A"]
- [ ] Document what went wrong for postmortem

## Monitoring Plan

### First 30 Minutes
- [ ] Watch application logs: [command — e.g., `flyctl logs -a [app]`]
- [ ] Check error rates: [dashboard URL or command]
- [ ] Verify health endpoint: `watch -n 30 curl -s https://[url]/health`
- [ ] Check response times: [dashboard URL or command]

### First 24 Hours
- [ ] Review error logs for new exception types
- [ ] Compare request latency against pre-deploy baseline
- [ ] Check memory/CPU usage trends
- [ ] Verify scheduled jobs still running (if applicable)
- [ ] Confirm database metrics stable (connection count, query time)

### Alerts
- [ ] Uptime monitor configured: [tool — e.g., UptimeRobot, Fly.io healthcheck]
- [ ] Error alerting configured: [tool — e.g., Sentry, Discord webhook]
- [ ] Resource alerting configured: [memory/CPU thresholds]

## Constraints
- Deploy only code that has passed CI
- Never deploy on Friday afternoon unless it is an emergency fix
- One deploy at a time — do not stack concurrent deployments
- Database migrations must be backward-compatible (old code must work with new schema)
- If smoke tests fail, roll back before investigating
- Document every manual step performed during deploy

## Required Output
1. Deploy confirmation (version/commit deployed, timestamp)
2. Smoke test results (all pass / failures noted)
3. Monitoring status after 30 minutes (healthy / issues found)
4. Rollback status (not needed / executed — with reason)
5. Follow-up items (monitoring to watch, cleanup to schedule)
```
