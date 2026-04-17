# Master Session Template

Use this at the start of any coding session to give the agent a stable operating frame.
Covers objective, scope, constraints, validation, and output structure -- the minimum context
needed for reliable, non-improvised work.

```md
# Session Context

## Objective
[What must be accomplished in this session? Be specific.]

## Repo / Project
- Name: [project name]
- Purpose: [what the project does]
- Primary language(s): [Python / TS / Go / etc.]
- Stack: [frameworks, infra, tools]
- Environment: [local / Docker / cloud / CI]

## Current Task
[Describe the exact task to perform.]

## Why This Task Matters
[Explain the operational or product reason this task exists.]

## Scope
### In scope
- [item]
- [item]

### Out of scope
- [item]
- [item]

## Constraints
- Do not break existing behavior unless explicitly required
- Follow existing project conventions
- Prefer small, reviewable changes
- Ask: what is the minimum correct change?
- If assumptions are needed, state them clearly

## Codebase Guidance
- Reuse existing patterns before introducing new abstractions
- Do not invent architecture without checking current repo structure
- Favor maintainability over cleverness
- Keep changes aligned with the project style

## Files / Areas Most Likely Relevant
- [path/file]
- [path/file]
- [path/file]

## Validation Requirements
- [tests to run]
- [lint/type checks]
- [manual verification steps]
- [CI expectations]

## Output Required
[What do you want back?]
Examples:
- code change
- patch
- plan + patch
- root cause analysis
- refactor proposal
- docs update
- test additions

## Deliverable Format
Return:
1. Summary of what you changed
2. Files changed
3. Risks / assumptions
4. Validation performed
5. Follow-up recommendations if needed

## Avoid
- large speculative refactors
- changing unrelated code
- silently ignoring edge cases
- placeholder logic unless explicitly requested

## Extra Context
[Any business logic, user expectations, deadlines, compatibility constraints, performance needs, etc.]
```
