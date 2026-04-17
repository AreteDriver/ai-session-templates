# Example Repo Review Session

A filled-in credibility review template for evaluating a developer tools repo before an interview or public positioning conversation.

```md
# Repo Review Session

## Repo
agent-lint — https://github.com/AreteDriver/agent-lint
A static analysis tool that audits AI agent workflow configurations (YAML) before they reach production.

## Purpose
Lint agent workflow definitions for structural issues, missing guardrails, unbounded cost exposure, and governance violations. Designed to run locally or as a CI gate.

## Review Goal
Interview narrative and recruiter credibility. I need to know exactly what this repo signals to a stranger — a hiring manager at Palantir or Scale AI who spends 30 seconds on the README, scans the CI badges, and decides whether to keep reading.

## Evaluate These Areas

### First Impression (30-second scan)
- Does the README communicate what this is and why it matters within 10 seconds?
- Are there badges (CI, coverage, PyPI, license)?
- Is there a clear install command and usage example above the fold?
- Does it look maintained or abandoned?

### README Quality
- Is the problem statement specific and credible?
- Are usage examples copy-pasteable and correct?
- Does it explain what the tool checks (rule categories, output format)?
- Is there a CI integration example?
- Does the tone match a real developer tool (not a tutorial project)?

### Test Coverage and Quality
- Total test count and coverage percentage
- Are tests organized by module (unit vs integration)?
- Do tests cover edge cases or just happy paths?
- Is there evidence of regression tests for fixed bugs?

### CI Signals
- GitHub Actions workflows present and passing?
- Does CI run tests, lint, type checks, and security scans?
- Is there a release/publish workflow (PyPI trusted publisher)?
- Are workflow files clean or cargo-culted?

### Release Hygiene
- Is the package on PyPI with a real version (not 0.0.1)?
- Is there a CHANGELOG or release notes?
- Are git tags consistent with PyPI versions?
- Is the packaging standard (pyproject.toml, not setup.py)?

### Architecture Coherence
- Is the code organized into clear layers (CLI, rules, reporting, config)?
- Are abstractions justified or premature?
- Is the rule system extensible without modifying core logic?
- Does the codebase match what the README claims?

### Weakest Points
- What would a skeptical reviewer attack first?
- Are there signs of AI-generated filler (empty docstrings, boilerplate without substance)?
- Is there anything that undermines credibility (dead code, TODO sprawl, broken links)?

## Required Output

### 1. 30-Second Impression
What this repo tells a stranger who lands on it cold. One paragraph.

### 2. Strengths
Bulleted list of what works well. Be specific — "good test coverage" is not useful. "296 tests at 98% coverage with edge case coverage for malformed YAML inputs" is useful.

### 3. Weaknesses
Bulleted list of what hurts credibility or signals inexperience. Be direct.

### 4. Top 5 Fixes by Impact
Ordered list. Each item: what to fix, why it matters, estimated effort (trivial / small / medium).

### 5. Positioning Language
Two to three sentences I could use in an interview or README to describe what this repo demonstrates about how I work. No hype. Grounded in what the repo actually contains.

## Context for Review
This repo is part of a portfolio of 5 monetized developer tools (agent-lint, anchormd, promptctl, context-hygiene, ai-spend) all sharing a license server on Fly.io. The interviewer may look at multiple repos, so consistency across the portfolio matters. The goal is not "impressive side project" — it is "evidence of production engineering discipline applied to AI tooling."
```
