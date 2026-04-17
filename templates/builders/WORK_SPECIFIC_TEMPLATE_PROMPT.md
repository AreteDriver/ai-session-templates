# Work-Specific Template Prompt

A meta-prompt for asking a coding agent to generate a new reusable session template. Use this when you have a repeating work pattern and want the model to draft a structured template you can refine and keep.

```md
Create a reusable session template for the following type of engineering work.

## Work Type
[insert work type — be specific]
Examples: "FastAPI endpoint addition", "Alembic migration authoring", "Discord bot command",
"PyPI package release", "Bevy ECS system", "MCP tool implementation"

## Context
- Repo: [name and purpose]
- Stack: [language, framework, infra]
- Typical tasks in this category: [describe 2-3 real examples]
- Common failure modes: [what usually goes wrong]

## The template must include these sections:
- **Objective**: what the session should accomplish
- **Scope**: in-scope and out-of-scope boundaries
- **Constraints**: rules the model must follow (repo conventions, behavior preservation, diff size)
- **Likely relevant files**: where to point the model
- **Risks / failure modes**: what to watch for
- **Validation requirements**: how to verify the work is correct
- **Output format**: what the model should deliver (summary, files changed, risks, validation)
- **What to avoid**: specific anti-patterns for this work type

## Requirements for the generated template:
- Make it practical, not generic — it should reflect how this work actually happens in this repo
- Optimize for repo-aware work — assume the model has access to the codebase
- Prefer small, reviewable changes over large rewrites
- Include real examples where they clarify intent
- Keep it concise enough to fill out in under 5 minutes
- The final template must be in a single markdown code block, copy-paste ready

## Quality check before delivering:
- Would this template produce better results than "help me with [work type]"?
- Does every section earn its space, or is it filler?
- Could someone unfamiliar with the work type use this template successfully?
- Are the constraints specific enough to prevent common model mistakes?
```
