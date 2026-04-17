# Community Templates

Templates contributed by engineers using this system in real projects.

## How to Submit

1. Fork this repo and create a branch.
2. Add your template as a new `.md` file in this directory.
3. Open a PR with a brief description of the work pattern it covers.

## Format Requirements

Every community template must include:

- An `H1` title describing the work pattern (e.g., `# Database Schema Design Template`)
- A 1-2 sentence description of when to use it
- The full template inside a fenced markdown codeblock (` ```md `)
- The template must include: objective, scope, constraints, validation, and output format sections

## Quality Bar

- Templates must come from real engineering work, not hypothetical scenarios.
- If you have not used it in at least two real sessions, it is not ready.
- Match the tone of existing templates: precise, practical, no filler.
- Include YAML frontmatter with `version` and `updated` fields.

## Naming

Use `SCREAMING_SNAKE_CASE.md` matching the pattern in `templates/workflows/` and `templates/custom/`.
