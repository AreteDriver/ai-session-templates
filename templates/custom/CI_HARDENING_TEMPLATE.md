# CI Hardening Template

For tightening CI/CD pipelines — adding missing quality gates, fixing fragile workflows, improving failure visibility, and making the release path reliable. Applies whether the stack is GitHub Actions, GitLab CI, or anything else with a pipeline concept.

```md
# CI Hardening Session

## Objective
[What CI/CD problem are we solving? Be specific.]
Examples:
- "tests pass locally but fail in CI due to missing env"
- "no security scanning — need gitleaks + dependency audit"
- "release process is manual — want tag-triggered publish"
- "CI is green but coverage is not enforced"

## Project Context
- Repo: [name + path]
- CI platform: [GitHub Actions / GitLab CI / CircleCI / etc.]
- Language: [Python / Node / Rust / Go / etc.]
- Package registry: [PyPI / npm / crates.io / Docker Hub / none]
- Current workflow files: [list paths, e.g., .github/workflows/ci.yml]

## Current State

### What Exists
- [ ] Lint step: [tool + config, or "none"]
- [ ] Test step: [framework + approximate coverage, or "none"]
- [ ] Type check: [mypy / pyright / tsc / "none"]
- [ ] Security scan: [gitleaks / CodeQL / bandit / snyk / "none"]
- [ ] Dependency audit: [dependabot / renovate / pip-audit / npm audit / "none"]
- [ ] Build verification: [package builds cleanly in CI, or "untested"]
- [ ] Release automation: [tag-triggered publish, or "manual"]
- [ ] Branch protection: [required checks, or "none"]

### What Triggers CI
- [ ] Push to main
- [ ] Pull requests
- [ ] Tags / releases
- [ ] Schedule (cron)
- [ ] Manual dispatch

### Known Problems
- [problem: what breaks, when, why]
- [problem: what breaks, when, why]

## Gaps

Identify what is missing or weak. Check all that apply.

### Quality Gates (not yet enforced in CI)
- [ ] Lint must pass before merge
- [ ] Tests must pass before merge
- [ ] Coverage must meet threshold (target: [X]%)
- [ ] Type checks must pass
- [ ] No known security vulnerabilities in dependencies
- [ ] No secrets in committed code
- [ ] Package builds successfully
- [ ] Docs build successfully (if applicable)

### Reliability Gaps
- [ ] Flaky tests (pass sometimes, fail sometimes)
- [ ] Missing caching (slow CI runs)
- [ ] Matrix gaps (not testing across required versions/platforms)
- [ ] Brittle workflow logic (hardcoded values, fragile conditions)
- [ ] No timeout protection (workflows can hang indefinitely)
- [ ] Secrets not properly scoped or rotated

### Release Gaps
- [ ] No automated release on tag
- [ ] No changelog generation
- [ ] No artifact verification (published package not tested post-publish)
- [ ] No rollback strategy documented
- [ ] OIDC trusted publishing not configured (PyPI / npm)

## Desired Gates

Define what the CI pipeline should enforce after hardening.

### On Every Push / PR
1. Lint: [tool] — must pass, no warnings
2. Test: [framework] — must pass, coverage >= [X]%
3. Type check: [tool] — must pass (if applicable)
4. Security: [gitleaks / bandit / etc.] — no findings
5. Build: package must build cleanly

### On Tag / Release
1. All push/PR gates pass
2. Package published to [registry]
3. GitHub Release created with changelog
4. Post-publish verification (install from registry, run smoke test)

### On Schedule (if applicable)
- Dependency audit: [weekly / daily]
- Security scan: [what tool, what scope]

## Workflow Design

### File Structure
```
.github/workflows/
  ci.yml          # push + PR: lint, test, type check, security
  release.yml     # tag-triggered: build + publish + GitHub Release
  audit.yml       # scheduled: dependency and security audit
```

### Caching Strategy
- Dependencies: [pip cache / node_modules / cargo registry]
- Build artifacts: [what to cache between runs]

### Matrix (if applicable)
- Python: [3.11, 3.12, 3.13]
- OS: [ubuntu-latest, macos-latest, windows-latest]
- Node: [18, 20, 22]

## Failure Handling
- Lint failure: blocks merge, developer must fix
- Test failure: blocks merge, shows which tests failed and why
- Coverage drop: blocks merge if below threshold (not on absolute number, on delta)
- Security finding: blocks merge, shows finding with remediation guidance
- Flaky test: identify and quarantine, do not let it block the pipeline silently
- Timeout: set explicit timeouts on every job (default: 15 minutes)

## Constraints
- Do not make workflows opaque — every step should have a clear purpose
- Prefer explicit failures over silent passes
- Do not introduce tools the team cannot maintain
- Keep workflow files readable — avoid excessive YAML nesting or reusable-workflow indirection unless genuinely needed
- Changes must be reviewable — no "rewrite the entire CI in one commit"
- Secret handling must follow least-privilege (scoped to jobs that need them)

## Required Output
1. Hardening summary (what was added, changed, or removed)
2. Updated workflow files (with inline comments explaining non-obvious choices)
3. Gap closure checklist (which gaps from above are now covered)
4. Remaining risks (what is still not covered and why)
5. Validation performed (syntax check, dry run, or actual CI run)
6. Rollback plan (how to revert if the new CI breaks something)

## What to Avoid
- Adding gates that nobody will maintain
- Cargo-culting security tools without understanding their output
- Making CI so slow that developers avoid running it
- Badge-driven development (adding badges for tools that are not actually enforced)
- Ignoring the human workflow — CI should help developers, not punish them
```
