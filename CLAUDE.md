# CLAUDE.md -- ai-session-templates

## Project Overview

Structured markdown template library for Claude Code, Codex, and repo-aware coding agents.
Not a prompt collection. A session-setup system for disciplined AI-assisted engineering.

## Architecture

```
ai-session-templates/
├── CLAUDE.md
├── README.md
├── LICENSE
├── docs/                    # Philosophy, usage, conventions
├── templates/
│   ├── master/              # Universal session template
│   ├── workflows/           # Common engineering work (bugfix, feature, refactor, CI, docs, review, research)
│   ├── builders/            # Meta-templates for creating new templates
│   └── custom/              # Higher-value specialized templates (credibility, CLI, AI workflow, OSS, CI hardening)
└── examples/                # Realistic filled-in sessions
```

## Conventions

- **Format**: All templates are markdown inside fenced codeblocks
- **Structure**: Brief description before codeblock, then the copy-paste-ready template
- **Naming**: SCREAMING_SNAKE_CASE.md for templates, lowercase for examples
- **Tone**: Precise, practical, engineer-facing. No hype, no filler, no emojis
- **Content**: Every template must include objective, scope, constraints, validation, and output format

## Common Commands

```bash
# Validate markdown links
for f in $(find . -name '*.md' -not -path './.git/*'); do
  grep -oP '\[.*?\]\(\K[^)#]+' "$f" 2>/dev/null | while read -r link; do
    echo "$link" | grep -qP '^https?://' && continue
    [ ! -e "$(dirname "$f")/$link" ] && echo "BROKEN: $f -> $link"
  done
done
```

## Git Conventions

- Conventional commits: `docs:`, `feat:`, `fix:`, `chore:`
- Work directly on `main`

## Anti-Patterns

- Do NOT add generic prompt-library content
- Do NOT use marketing language or inflated claims
- Do NOT create empty placeholder files
- Do NOT duplicate content across templates
- Templates must reflect real engineering workflows, not theoretical ones
