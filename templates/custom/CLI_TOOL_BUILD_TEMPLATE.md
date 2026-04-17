---
version: "1.0.0"
updated: "2026-04-17"
---

# CLI Tool Build Template

For building command-line tools from scratch or extending existing ones. Covers the full surface: commands, I/O, error handling, packaging, docs, tests, and CI. Keeps the build grounded in user need rather than feature creep.

```md
# CLI Tool Build Session

## Tool Purpose
[One sentence: what does this CLI do and why does it exist?]

## User Need
- Who runs this tool? [developer / operator / CI pipeline / end user]
- What problem does it solve for them?
- What are they doing before this tool exists? [manual steps / scripts / nothing]

## Project Context
- Repo: [name + path]
- Language: [Python / Go / Rust / Node / etc.]
- Package manager: [pip / cargo / npm / etc.]
- Entry point pattern: [click / typer / argparse / cobra / clap / etc.]
- Existing CLI patterns in repo: [describe or "none"]

## Commands and Subcommands

Define the command surface. Be explicit about what each command does.

| Command | Arguments | Description |
|---------|-----------|-------------|
| [cmd] | [args/flags] | [what it does] |
| [cmd] | [args/flags] | [what it does] |

### Global Flags
- `--verbose` / `-v`: [increase output detail]
- `--quiet` / `-q`: [suppress non-error output]
- `--output` / `-o`: [output format: text / json / table]
- [other global flags]

## Input / Output Format

### Input
- stdin: [yes/no, what format]
- file arguments: [paths, globs, config files]
- environment variables: [list any expected env vars]
- config file: [path and format if applicable]

### Output
- stdout: [what goes here — results, data, reports]
- stderr: [what goes here — errors, warnings, progress]
- exit codes: [0 = success, 1 = error, 2 = usage error, etc.]
- file output: [if the tool writes files, describe format and location]

## Error Handling
- invalid input: [how to report — specific message + exit code]
- missing dependencies: [check at startup or fail on use]
- partial failures: [continue with warnings or abort]
- network errors: [if applicable — timeout, retry, fallback]
- permission errors: [file access, API auth]

Design principle: errors should be specific enough that the user knows what to fix without reading source code.

## Packaging and Distribution
- Install method: [pip install / cargo install / npm -g / brew / binary release]
- Entry point name: [what the user types to invoke]
- Dependencies: [runtime deps, keep minimal]
- Python-specific: [pyproject.toml, console_scripts entry point]
- Binary-specific: [cross-compilation targets, release artifacts]

## Documentation
- `--help` output: [must be clear without reading docs]
- README section: [install + usage + examples]
- Man page: [if applicable]
- Usage examples: [at least 3 realistic examples in README or --help]

## Tests
- Unit tests: [core logic, argument parsing, output formatting]
- Integration tests: [actual CLI invocation via subprocess or click.testing]
- Edge cases: [empty input, malformed input, missing files, permission denied]
- Coverage target: [80%+ recommended]

## CI Integration
- Lint: [ruff / clippy / eslint]
- Test: [pytest / cargo test / jest]
- Type check: [mypy / pyright if applicable]
- Package build: [verify installable artifact builds cleanly]
- Release: [tag-triggered publish to PyPI / npm / crates.io / GitHub Releases]

## Scope

### In scope
- [feature]
- [feature]

### Out of scope
- [feature — and why it is excluded]
- [feature — and why it is excluded]

## Constraints
- Keep the interface simple — fewer commands done well beats many done poorly
- No hidden behavior — every side effect should be visible or opt-in
- Align with existing repo conventions
- Prefer sensible defaults with explicit overrides
- Include docs and examples as part of the build, not as follow-up work

## Required Output
1. Implementation summary (what was built)
2. Command reference with examples
3. Files created or changed
4. Test results and coverage
5. Packaging verification (installable, entry point works)
6. Risks, tradeoffs, and recommended next steps

## What to Avoid
- flag soup — too many flags nobody will remember
- clever aliases that obscure intent
- silent failures or swallowed errors
- output that is only readable by the developer who wrote it
- shipping without --help being useful on its own
```
