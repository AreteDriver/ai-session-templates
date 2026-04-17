# OSS Positioning Template

For framing a repository so it reads as credible, purposeful, and adoptable to its target audience. This is not about marketing. It is about making the repo's value legible to someone who has thirty seconds and no prior context.

```md
# OSS Positioning Session

## Project Identity
- Repo: [name + link]
- One-line description: [what it does, for whom, in one sentence]
- Category: [CLI tool / library / framework / application / infrastructure / reference]
- Stage: [prototype / alpha / beta / stable / maintenance]
- License: [MIT / Apache-2.0 / etc.]

## Target Audience
Who is this repo for? Be specific — "developers" is not an audience.
- Primary: [e.g., "Python developers building MCP servers"]
- Secondary: [e.g., "hiring managers evaluating AI engineering portfolios"]
- Tertiary: [e.g., "OSS contributors looking for well-maintained projects"]

### What Each Audience Cares About
| Audience | They want to know | They will check |
|----------|------------------|-----------------|
| [audience] | [question they have] | [what they look at] |
| [audience] | [question they have] | [what they look at] |

## Differentiation
- What does this repo do that alternatives do not?
- What is the specific problem framing that makes it distinct?
- Is the differentiation in the approach, the scope, the quality, or the domain?
- Can you state the differentiation in one sentence without superlatives?

### Comparison (if applicable)
| Feature | This project | Alternative A | Alternative B |
|---------|-------------|--------------|--------------|
| [feature] | [status] | [status] | [status] |

## README Structure

A well-positioned README follows this order. Fill in what exists and what is missing.

### 1. Opening (above the fold)
- [ ] One sentence: what it does
- [ ] One sentence: why it exists
- [ ] One sentence: who it is for
- [ ] Badge row (only meaningful badges: CI status, coverage, PyPI/npm version, license)

### 2. Quick Start
- [ ] Install command (copy-paste ready)
- [ ] Minimal working example (under 10 lines)
- [ ] Expected output shown

### 3. Why This Exists
- [ ] Problem statement (what is painful without this)
- [ ] Approach (how this solves it differently)
- [ ] Not a feature list — a motivation section

### 4. Features / Capabilities
- [ ] Bulleted, scannable
- [ ] Each feature grounded in user benefit, not implementation detail

### 5. Usage Examples
- [ ] At least 3 realistic examples
- [ ] Examples show common workflows, not edge cases
- [ ] Code blocks with language hints

### 6. Architecture (if applicable)
- [ ] High-level diagram or description
- [ ] Where to start reading the code
- [ ] Key abstractions explained

### 7. Contributing / Development
- [ ] How to set up local dev
- [ ] How to run tests
- [ ] What kind of contributions are welcome

### 8. Status and Roadmap
- [ ] Current state (honest)
- [ ] What is stable vs. experimental
- [ ] Planned next steps (if any)

## Proof Signals

These are the things that make a repo look real vs. generated.

### Strong Signals (have these)
- [ ] Tests with meaningful coverage (not just "tests exist")
- [ ] CI that passes on the default branch
- [ ] Versioned releases (tags, changelog, or release notes)
- [ ] Commit history showing real iteration (not one bulk commit)
- [ ] Issues or PRs that show the project is alive
- [ ] README examples that actually work

### Weak Signals (avoid these)
- [ ] Badge walls with no substance behind them
- [ ] "Coming soon" sections that never ship
- [ ] Marketing language ("revolutionary", "blazing fast", "enterprise-grade")
- [ ] AI-generated README that does not match the actual code
- [ ] Feature lists with no evidence of implementation
- [ ] Empty or boilerplate CONTRIBUTING.md

## What to Avoid
- Overselling: claim what the repo does today, not what it might do
- Underselling: do not hide real accomplishments behind false modesty
- Jargon walls: if the README requires domain expertise to parse the first paragraph, the positioning has failed
- Feature-first framing: lead with the problem and the user, not the implementation
- Comparing to well-known projects unless the comparison is honest and specific

## Required Output
1. Current positioning assessment (how the repo reads today, cold)
2. Target positioning statement (1-3 sentences for the README opening)
3. Strongest existing proof signals (what to keep and amplify)
4. Weakest signals (what to fix or remove)
5. README structure gaps (what sections are missing or weak)
6. Concrete fixes ordered by impact (what to change first)
7. Positioning language for external use (portfolio, interview, social)
```
