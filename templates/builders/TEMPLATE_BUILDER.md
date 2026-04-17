---
version: "1.0.0"
updated: "2026-04-17"
---

# Template Builder

Use this when none of the existing templates fit the work. This is the template for creating a new reusable session template. It walks through the design decisions so the resulting template is practical, not generic.

```md
# Template Builder

## Work Type
[What kind of engineering session is this template for?]
Examples:
- data migration
- API integration
- performance tuning
- security review
- test generation
- package publishing
- internal tooling
- eval harness work
- multi-agent workflow design
- infrastructure provisioning
- incident response

## Main Goal
[What does a successful session using this template produce?]
Be specific — "working code" is not a goal. "A tested migration script that moves user data from SQLite to PostgreSQL with rollback support" is.

## Typical Inputs
What information does the model need to do this work well?
- [ ] Repo path and structure
- [ ] Relevant source files
- [ ] Requirements or specifications
- [ ] Example inputs / outputs
- [ ] Schemas or data models
- [ ] Logs or error output
- [ ] Existing patterns to follow
- [ ] Constraints (performance, compatibility, cost)
- [ ] [other]

## Typical Risks
What usually goes wrong in this type of work?

### Model Behavior Risks
- [ ] Generates plausible but incorrect logic
- [ ] Invents abstractions that do not fit the codebase
- [ ] Ignores edge cases unless explicitly listed
- [ ] Produces verbose code when minimal change is needed
- [ ] Hallucinates API signatures or library behavior
- [ ] Silently changes behavior outside the requested scope

### Engineering Risks
- [ ] Incomplete migration / partial state
- [ ] Breaking existing tests or behavior
- [ ] Security regressions
- [ ] Performance regressions
- [ ] Dependency conflicts
- [ ] [other]

## Required Constraints
What rules must the template enforce?
- [ ] Must follow existing repo patterns
- [ ] Must preserve current behavior unless explicitly changing it
- [ ] Must stay within defined scope
- [ ] Must produce reviewable diffs (no bulk rewrites)
- [ ] Must validate output (tests, lint, type check)
- [ ] Must state assumptions explicitly
- [ ] Must not introduce unnecessary dependencies
- [ ] [other]

## Required Output Structure
What should the model deliver at the end of a session using this template?
1. Summary of what was done
2. Files created or changed
3. Assumptions made
4. Risks or tradeoffs
5. Validation performed
6. Recommended follow-up

## Validation Expectations
How do you know the work is correct?
- [ ] Existing tests pass
- [ ] New tests added for new behavior
- [ ] Lint / format clean
- [ ] Type check passes
- [ ] Manual verification steps: [describe]
- [ ] CI green
- [ ] [other]

## Template Draft

Now write the actual reusable template below. It should be:
- Copy-paste ready
- Concise enough to fill out in under 5 minutes
- Specific enough to produce better results than a generic prompt
- Structured with: objective, scope, constraints, relevant files, validation, output format, what to avoid

---

[Write the final template here in markdown format]
```
