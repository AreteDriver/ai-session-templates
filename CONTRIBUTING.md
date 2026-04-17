# Contributing

## How to Contribute Templates

1. Fork the repo and create a feature branch.
2. Add your template to `templates/community/` (or propose additions to `workflows/` / `custom/`).
3. Open a PR describing the work pattern and why the existing templates do not cover it.

## Style Guide

- Match the tone of existing templates: direct, practical, engineer-facing.
- No marketing language, no filler, no emojis.
- Every template must include: objective, scope, constraints, validation, and output format.
- Wrap the copy-paste-ready template in a fenced ` ```md ` codeblock.
- Use `SCREAMING_SNAKE_CASE.md` for template filenames.
- Include YAML frontmatter at the top of every template file:
  ```yaml
  ---
  version: "1.0.0"
  updated: "YYYY-MM-DD"
  ---
  ```

## Template Quality Bar

- Must come from real engineering sessions, not hypothetical workflows.
- Should have been used in at least two real sessions before submission.
- Must not duplicate an existing template. If it overlaps, explain why the existing one is insufficient.

## PR Process

1. One template per PR (unless they are closely related).
2. Include a brief description of the work pattern in the PR body.
3. If adding to `workflows/` or `custom/`, explain why `community/` is not the right home.
4. PRs are reviewed for clarity, completeness, and adherence to the style guide.

## Other Contributions

- Bug fixes to docs or examples: open a PR with a clear description.
- New examples: add to `examples/` with lowercase naming and a realistic filled-in session.
- Improvements to existing templates: explain what changed and why in the PR description.

## Commit Style

Use conventional commits: `docs:`, `feat:`, `fix:`, `chore:`.
