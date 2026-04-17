---
version: "1.0.0"
updated: "2026-04-17"
---

# Security Review Template

For structured security audits and hardening sessions. Covers OWASP Top 10, dependency vulnerabilities,
secrets scanning, auth/authz review, and infrastructure configuration. Produces actionable findings
with severity ratings and concrete fixes.

```md
# Security Review Session

## Scope
- Repo: [name + path]
- Language: [Python / Node / Rust / Go / etc.]
- Framework: [FastAPI / Django / Express / Next.js / etc.]
- Deployment: [Fly.io / Vercel / AWS / bare metal / local only]
- Auth mechanism: [JWT / session cookies / OAuth2 / API keys / none]
- Last security review: [date or "never"]
- Review depth: [full audit / targeted area / dependency-only / pre-deploy check]

## Threat Model Context
- Application type: [public web app / internal API / CLI tool / library / background service]
- Data sensitivity: [PII / financial / credentials / public only]
- User-facing: [yes / no]
- Internet-exposed: [yes / no — list public endpoints]
- Trust boundaries: [where untrusted input enters the system]
- Known threat actors: [opportunistic scanners / targeted / insider / none identified]

## Assets at Risk
| Asset | Sensitivity | Impact if Compromised |
|-------|------------|----------------------|
| [user data / API keys / database / sessions] | [high / medium / low] | [description] |

## Review Checklist

### A1: Injection
- [ ] SQL injection: parameterized queries used everywhere (no string interpolation in SQL)
- [ ] Command injection: no `os.system()`, `subprocess.shell=True` with user input
- [ ] Path traversal: file paths validated, no user-controlled `../` sequences
- [ ] Template injection: user input not passed raw to template engines
- [ ] LDAP/NoSQL injection: query construction uses safe builders

### A2: Broken Authentication
- [ ] Passwords hashed with bcrypt/argon2 (not MD5/SHA1)
- [ ] Session tokens are cryptographically random, sufficient length
- [ ] JWT: algorithm pinned (no `alg: none`), expiry enforced, secret rotated
- [ ] Brute force protection: rate limiting on login endpoints
- [ ] Multi-factor: available for privileged accounts (if applicable)
- [ ] Password reset: token expires, single-use, no information leakage

### A3: Sensitive Data Exposure
- [ ] HTTPS enforced (HSTS header set)
- [ ] Sensitive data not logged (passwords, tokens, PII)
- [ ] Sensitive data not in URL parameters
- [ ] Database encryption at rest (if required by data classification)
- [ ] Secrets not in source code, env vars, or config files committed to git

### A4: XML External Entities (XXE)
- [ ] XML parsing disabled or configured to reject external entities
- [ ] If XML accepted: DTD processing disabled, external entity resolution disabled

### A5: Broken Access Control
- [ ] Authorization checked on every protected endpoint (not just authentication)
- [ ] No IDOR: object access verified against requesting user
- [ ] Admin endpoints require elevated privileges
- [ ] CORS configured restrictively (not `*` in production)
- [ ] Directory listing disabled on static file servers

### A6: Security Misconfiguration
- [ ] Debug mode off in production
- [ ] Default credentials removed
- [ ] Stack traces not exposed to users (error responses are generic)
- [ ] Security headers set: CSP, X-Content-Type-Options, X-Frame-Options, Referrer-Policy
- [ ] Unnecessary HTTP methods disabled (TRACE, OPTIONS if not needed)
- [ ] Server version headers suppressed

### A7: Cross-Site Scripting (XSS)
- [ ] Output encoding applied to all user-controlled content in HTML
- [ ] Content-Security-Policy header restricts inline scripts
- [ ] DOM-based XSS: no `innerHTML` / `dangerouslySetInnerHTML` with user input
- [ ] Stored XSS: database content sanitized before rendering

### A8: Insecure Deserialization
- [ ] No `pickle.loads()` / `yaml.load()` (unsafe loader) on untrusted data
- [ ] JSON preferred over binary serialization for external input
- [ ] Deserialized objects validated before use

### A9: Using Components with Known Vulnerabilities
- [ ] `pip-audit` / `npm audit` / `cargo audit` run — no critical/high findings
- [ ] Dependabot or Renovate enabled
- [ ] No pinned dependencies with known CVEs
- [ ] Transitive dependency vulnerabilities reviewed

### A10: Insufficient Logging and Monitoring
- [ ] Authentication events logged (success and failure)
- [ ] Authorization failures logged
- [ ] Input validation failures logged (potential attack indicators)
- [ ] Logs do not contain secrets or PII
- [ ] Log retention and access controls defined

### Additional Checks
- [ ] CSRF protection on state-changing endpoints (if cookie-based auth)
- [ ] Rate limiting on public API endpoints
- [ ] File upload validation (type, size, content inspection)
- [ ] WebSocket authentication and authorization (if applicable)
- [ ] Secrets scanning: no API keys, tokens, or passwords in repository history

## Tooling
Run these and record results:

| Tool | Purpose | Command |
|------|---------|---------|
| gitleaks | Secrets in git history | `gitleaks detect --source . -v` |
| bandit | Python static analysis | `bandit -r . -f json` |
| pip-audit | Python dependency CVEs | `pip-audit` |
| npm audit | Node dependency CVEs | `npm audit --production` |
| CodeQL | Semantic code analysis | GitHub Actions (if public repo) |
| semgrep | Pattern-based static analysis | `semgrep --config auto .` |
| trivy | Container image scan | `trivy image [image:tag]` |

## Constraints
- Do not introduce security tools the team cannot maintain
- Findings must include concrete fix, not just "this is bad"
- False positives must be documented and suppressed with rationale
- Do not weaken existing security controls to fix other issues
- Secrets found in history require rotation, not just removal from HEAD

## Required Output

### Findings Table
| # | Severity | Category | Location | Description | Fix |
|---|----------|----------|----------|-------------|-----|
| 1 | [critical/high/medium/low/info] | [OWASP category] | [file:line] | [what is wrong] | [how to fix it] |

### Summary
1. Total findings by severity: [critical: N, high: N, medium: N, low: N, info: N]
2. Top 3 risks requiring immediate action
3. Dependency audit results (clean / N vulnerabilities found)
4. Secrets scan results (clean / N findings — rotation needed)
5. Hardening recommendations (security headers, rate limiting, logging)
6. Items deferred and why (out of scope, acceptable risk, needs more investigation)
```
