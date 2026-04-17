---
version: "1.0.0"
updated: "2026-04-17"
---

# Proof of Method Template

For demonstrating engineering method through a repo or body of work. Used when the goal is not just shipping code but proving that the process behind the code is disciplined, repeatable, and worth trusting. Applies to interview prep, portfolio positioning, and team credibility.

```md
# Proof of Method Session

## What You Are Proving
[State the claim your work should support.]
Examples:
- "I can operationalize AI-assisted development without losing engineering discipline"
- "I build tools with CI, tests, packaging, and docs — not just scripts"
- "I can take a problem from definition through shipping with measurable quality"
- "I treat repos as products, not throwaway experiments"

## Target Audience
- Who needs to believe this claim? [hiring manager / interviewer / peer engineer / OSS community / client]
- What would convince them? [evidence they can verify in under 5 minutes]
- What would make them skeptical? [common red flags you need to avoid]

## Evidence Checklist

### Process Evidence (how the work was done)
- [ ] Commit history shows iterative development, not bulk dumps
- [ ] Issues or PRs show task decomposition and progress tracking
- [ ] Branching and merging show disciplined workflow (or justified trunk-based)
- [ ] CI runs show consistent green, not perpetual failures
- [ ] Release history shows versioned, intentional shipping

### Quality Evidence (how good the work is)
- [ ] Tests exist and cover meaningful behavior (not just happy path)
- [ ] Coverage is measured and reported (threshold enforced in CI)
- [ ] Linting and formatting are automated (not manual cleanup)
- [ ] Security scanning is present (gitleaks, dependabot, CodeQL, bandit)
- [ ] Dependencies are managed and up to date

### Documentation Evidence (how the work is communicated)
- [ ] README explains what, why, and how in under 60 seconds
- [ ] Install / setup instructions work on first try
- [ ] Usage examples are realistic and copy-paste ready
- [ ] Architecture is explained or inferable from structure
- [ ] CLAUDE.md / context files show awareness of AI-assisted workflow

### Operational Evidence (how the work runs)
- [ ] The tool / app / library actually works when installed fresh
- [ ] Error handling is specific and helpful
- [ ] Configuration is documented and has sensible defaults
- [ ] Performance is adequate for the stated use case
- [ ] Deployment is automated or clearly documented

### Engineering Judgment Evidence (decisions, not just output)
- [ ] Scope is constrained — the project does one thing well
- [ ] Architecture is appropriate to the problem size (not over-engineered)
- [ ] Tradeoffs are documented or visible in commit messages
- [ ] Known limitations are stated honestly
- [ ] There is evidence of iteration and improvement over time

## Narrative Structure

Build the story around these beats:

### 1. The Problem
- What specific problem does this work address?
- Why does it matter to someone other than the author?

### 2. The Approach
- What decisions were made and why?
- What was deliberately excluded and why?
- How was AI used (if applicable) — as accelerator, not replacement?

### 3. The Evidence
- What can the audience verify themselves?
- Point to specific artifacts: test results, CI runs, release tags, commit history

### 4. The Outcome
- What exists now that did not before?
- What is the measurable result? (coverage %, test count, deploy frequency, user adoption)

### 5. The Iteration
- How has the work improved over time?
- What was learned and applied?
- What would be done differently next time?

## Repo-Specific Proof Points

For each repo you want to use as evidence, fill in:

### Repo: [name]
- Purpose: [one line]
- Best proof signals: [what makes this repo credible]
- Weakest signals: [what could undermine the narrative]
- Talking points: [2-3 sentences for verbal explanation]
- Quick demo path: [what to show in 2 minutes if asked]

## Constraints
- Only claim what the evidence supports
- Do not conflate volume with quality — 10 well-built repos beat 50 toy scripts
- AI usage should be framed as disciplined leverage, not dependency
- Every claim should have a verifiable artifact
- Avoid "I built this in a weekend" framing — it signals low investment

## Required Output
1. Method statement (2-3 sentences describing the engineering approach)
2. Evidence map (which repos / artifacts support which claims)
3. Gap analysis (where the evidence is thin and what to do about it)
4. Talking points (interview-ready statements grounded in specifics)
5. Quick demo scripts (what to show, in what order, for maximum impact)
6. Anti-patterns to avoid (what would undermine the proof)

## What to Avoid
- Claiming the AI did the hard parts while you "directed"
- Presenting quantity of repos as proof of skill
- Showing repos that are clearly AI-generated boilerplate
- Overstating production usage when it is personal tooling
- Treating the portfolio as complete — always show active improvement
```
