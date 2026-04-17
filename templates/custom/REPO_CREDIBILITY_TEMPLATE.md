---
version: "1.0.0"
updated: "2026-04-17"
---

# Repo Credibility Template

For interview prep, OSS polish, or recruiter-facing credibility audits. Forces a structured review of how a repo reads to someone who has never seen it before and will spend less than two minutes deciding if the author is serious.

```md
# Repo Credibility Review

## Repo
- Name: [project name]
- Link: [GitHub URL or local path]
- Purpose: [one sentence — what it does and for whom]

## Audience
Who will judge this repo? Check all that apply.
- [ ] recruiter (scanning for signal, not reading code)
- [ ] hiring engineer (reading code, checking discipline)
- [ ] interviewer (looking for talking points and proof of depth)
- [ ] OSS user (evaluating whether to adopt or contribute)
- [ ] investor / partner (assessing technical credibility)

## Review Goal
[What outcome are you optimizing for?]
Examples:
- interview narrative strength
- OSS adoption readiness
- recruiter first-impression quality
- product credibility for a landing page

## Review Dimensions

Evaluate each area. Score 1-5 and note specifics.

### First Impression (30 seconds cold)
- What does a stranger see in the first scroll?
- Is the purpose immediately clear?
- Does it look maintained or abandoned?
- Are there obvious red flags (empty README, no commits in months, boilerplate)?

### README Quality
- Does it explain what, why, and who?
- Is there a quick-start that actually works?
- Are there usage examples that feel real, not generated?
- Is the tone precise or inflated?
- Does it show the project's current state honestly?

### Proof of Execution
- Are there tests? What is the coverage?
- Is there CI? Does it pass?
- Are there releases / tags / versioned artifacts?
- Is there a changelog or commit history that shows real iteration?
- Are issues / PRs used, or is the repo a monologue?

### Release and CI Hygiene
- Are GitHub Actions / CI workflows present and green?
- Is there a release process (tags, PyPI, npm, Docker)?
- Are dependencies managed (lockfile, dependabot, renovate)?
- Is there security scanning (gitleaks, CodeQL, bandit)?

### Architecture Coherence
- Does the code structure make sense on first read?
- Are patterns consistent or is it a patchwork?
- Is there unnecessary abstraction or premature optimization?
- Would a new contributor know where to start?

### Trustworthiness
- Are claims in the README backed by repo evidence?
- Are there inflated badge walls or marketing language?
- Does the commit history show real work or bulk-generated files?
- Is there anything that looks AI-generated-and-not-reviewed?

### Novelty and Differentiation
- Does this repo do something others do not?
- Is the problem framing clear and specific?
- Is there a reason to choose this over alternatives?
- Does it show engineering judgment, not just code volume?

## Constraints
- Evaluate what exists, not what was intended
- Do not give credit for planned features
- Judge from the perspective of the stated audience
- Be specific — "README is weak" is not useful; "README has no usage examples and the install section assumes local dev only" is useful

## Required Output

### 1. 30-Second Impression
[What does this repo communicate to a stranger in one scroll? Be blunt.]

### 2. Strengths
[Specific credibility signals. What works and why.]

### 3. Weaknesses
[Specific gaps. What hurts credibility and why.]

### 4. Top Fixes by Impact
[Ordered list of changes that would most improve the repo's signal. Focus on highest leverage first.]

### 5. Positioning Language
[Draft 2-3 sentences the author could use to describe this repo in an interview, README, or portfolio. Grounded in evidence, not aspiration.]

## What to Avoid
- inflated praise
- vague feedback ("looks good" / "needs work")
- suggestions that require a full rewrite
- ignoring the stated audience
- treating badge count as a quality signal
```
