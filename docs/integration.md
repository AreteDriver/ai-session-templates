# Integration Guide

How to wire session templates into your Claude Code workflow.

## Option 1: Manual Copy-Paste (Simplest)

1. Open the template file from `templates/`.
2. Copy the markdown codeblock contents into your session prompt.
3. Fill in the bracketed fields.
4. Paste into the coding agent.

Works with any agent. No setup required. Best for occasional use.

## Option 2: CLAUDE.md Reference (Recommended)

Add the following to your project's `CLAUDE.md`:

```markdown
## Session Templates

Before starting any engineering session, select and fill the appropriate template from:
https://github.com/AreteDriver/ai-session-templates/tree/main/templates

Template selection:
- Bug/error: templates/workflows/BUGFIX_TEMPLATE.md
- New feature: templates/workflows/FEATURE_BUILD_TEMPLATE.md
- Refactor: templates/workflows/REFACTOR_TEMPLATE.md
- Deploy: templates/custom/DEPLOY_TEMPLATE.md
- Unclear: templates/master/MASTER_SESSION_TEMPLATE.md

Always require structured output: summary, files changed, risks, validation.
```

This gives every Claude Code session awareness of the templates without manual effort.

## Option 3: Session-Start Skill (Automated)

If you use the `ai-skills` repo, the session-start skill can auto-suggest the right template based on your stated objective.

1. Install the skill per `ai-skills` repo instructions.
2. At session start, state your objective.
3. The skill matches it to a template and pre-fills what it can.

Best for teams that want consistent session setup without relying on individual discipline.

## Option 4: Hook-Based (Advanced)

Use a Claude Code `PreToolUse` hook to suggest templates when a session starts without one.

Add to your `.claude/settings.json`:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash|Edit|Write",
        "command": "echo 'Reminder: select a session template from templates/ before writing code.'"
      }
    ]
  }
}
```

This fires a reminder before the first tool use in a session. It does not block execution -- it nudges toward structured setup.

## Which Option to Pick

| Situation | Option |
|-----------|--------|
| Solo, occasional use | Option 1 (copy-paste) |
| Solo, daily use | Option 2 (CLAUDE.md) |
| Team standardization | Option 3 (skill) |
| Enforcement without friction | Option 4 (hook) |

## Per-Session Toolkit

The session templates define structure. These tools add instrumentation -- measuring context quality, output quality, cost, and cross-session continuity. All are optional. The session-start and session-end skills in `ai-skills` gracefully skip any tool that is not installed.

| Tool | Phase | What it does | Install |
|------|-------|-------------|---------|
| anchormd | start | Audits CLAUDE.md quality (0-100 score) | `pip install anchormd` |
| memboot | start | Local codebase context retrieval | `pip install memboot` |
| fleet-monitor | start | Service health checks before you begin | Project-specific |
| Animus MCP | start + end | Persistent memory and task tracking across sessions | MCP server config |
| context-hygiene | mid-session | Measures context window bloat | `pip install context-hygiene` |
| drift-monitor | mid-session | Detects output quality degradation over long sessions | `pip install driftmonitor` |
| signal-audit | end | Scores AI output for signal-to-noise ratio | `pip install signal-audit` |
| content-scrubber | end | Removes AI-generated patterns from content | Claude Code skill |
| ai-spend | end | Reports token usage and cost per session | `pip install ai-spend` |
| promptctl | end | Versions and stores effective prompts | `pip install promptctlai` |

### Where to start

None of these are required to use the templates effectively. If you want to add instrumentation incrementally, two tools deliver the most immediate value:

1. **anchormd** -- measures the quality of your CLAUDE.md, which directly controls how well the agent understands your project. A weak CLAUDE.md means every session starts at a disadvantage. This is the single highest-leverage improvement for solo developers.

2. **signal-audit** -- scores the output you receive from the agent. Without measurement, you cannot tell if your templates and prompts are actually improving results. This closes the feedback loop.

### For teams

If you run multiple services or coordinate across sessions, two additional tools are worth the setup cost:

- **fleet-monitor** adds service-awareness to session start. The agent knows which services are healthy before it begins work, avoiding wasted effort on broken dependencies.
- **Animus MCP** provides cross-session memory. Decisions, patterns, and task state persist between sessions instead of being re-discovered each time. This compounds -- the tenth session is significantly more productive than the first.
