# Naming Conventions

## File Naming

All template files use uppercase snake case with a `_TEMPLATE` suffix:

```
BUGFIX_TEMPLATE.md
CI_HARDENING_TEMPLATE.md
REPO_CREDIBILITY_TEMPLATE.md
```

Builder files use uppercase snake case without the `_TEMPLATE` suffix when the file is a meta-tool rather than a fill-in template:

```
TEMPLATE_BUILDER.md
WORK_SPECIFIC_TEMPLATE_PROMPT.md
```

Doc files use lowercase kebab case:

```
usage-guide.md
best-practices.md
naming-conventions.md
```

Example files use lowercase snake case with an `example_` prefix:

```
example_bugfix_session.md
example_feature_session.md
```

## Naming Rules

1. Names must describe the work type, not the aspiration. `CI_HARDENING_TEMPLATE.md` tells you what it is for. `AWESOME_PIPELINE_TEMPLATE.md` does not.
2. Keep names short but unambiguous. If two names could be confused, make them more specific.
3. Do not use version numbers in filenames. Version through git, not filename suffixes.
4. Do not abbreviate unless the abbreviation is universally understood (`CI`, `API`, `CLI`, `OSS`).

## Folder Rules

### `templates/master/`
The single master session template. This is the general-purpose fallback when no specific template fits.

One file only: `MASTER_SESSION_TEMPLATE.md`

### `templates/workflows/`
Common engineering tasks that are broadly reusable across projects and teams.

Criteria for inclusion:
- The work type is something most developers do regularly
- The template does not assume a specific domain or tech stack
- Examples: bug fix, feature build, refactor, CI automation, docs hardening, repo review, research comparison

### `templates/custom/`
Specialized or high-value work patterns that reflect a specific operating style or domain need.

Criteria for inclusion:
- The work type is repeated but more niche than a standard workflow
- The template encodes domain knowledge or specific anti-patterns
- Examples: repo credibility audit, CLI tool build, AI workflow design, OSS positioning, CI hardening, proof of method

### `templates/builders/`
Meta-templates and prompts for generating new templates.

Criteria for inclusion:
- The file helps create or evaluate other templates
- It is a tool for extending the library, not a session template itself

### `examples/`
Filled-in examples showing how templates are used in practice.

Criteria for inclusion:
- Must be realistically filled out, not placeholder text
- Must demonstrate the level of specificity expected
- One example per major workflow type is sufficient

## When to Create a New File vs. Edit an Existing One

**Create a new template** when:
- The work type has distinct scope, risks, or validation that existing templates do not cover
- Editing an existing template to fit requires changing more than half the sections

**Edit an existing template** when:
- The improvement applies to the existing work type
- The change tightens constraints, adds validation, or improves structure without changing the template's purpose

**Do not create** a new template that differs from an existing one only in wording preference or minor structural rearrangement. That is duplication, not extension.
