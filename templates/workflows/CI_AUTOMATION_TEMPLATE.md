---
version: "1.0.0"
updated: "2026-04-17"
---

# CI / Automation Template

Use this when working on GitHub Actions, deployment scripts, release flows, or any pipeline
automation. Covers current vs. desired state, workflow areas, and failure considerations.

```md
# CI / Automation Session

## Objective
[What automation or pipeline improvement is needed?]

## Current State
[What exists now?]

## Desired State
[What should happen after this change?]

## Pipeline / Workflow Area
- build
- test
- lint
- package
- release
- deploy
- security scan
- artifact generation

## Constraints
- keep secrets safe
- avoid brittle workflow logic
- prefer observable failures
- document assumptions
- do not break current release flow without stating it clearly

## Validation
- syntax validity
- workflow logic review
- dry-run if possible
- required triggers / branches
- rollback or failure considerations
```
