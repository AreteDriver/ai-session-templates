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
