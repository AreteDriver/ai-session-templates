# AI Session Templates

A markdown-based template library for Codex, Claude Code, and other repo-aware coding agents.

This repo is designed to improve engineering sessions by forcing the right context up front:
objective, scope, constraints, likely files, validation, and output format.

The goal is not to collect prompts.
The goal is to make AI-assisted engineering work more reliable, reviewable, and reusable.

## Who this is for

- engineers using coding agents in real repos
- builders who want cleaner session setup
- operators who care about scope, validation, and maintainability
- anyone tired of vague “help me with this repo” prompts

## Repo structure

```text
docs/
  overview.md                  # philosophy and design principles
  usage-guide.md               # how to pick, fill, and use templates
  best-practices.md            # session discipline and anti-patterns
  naming-conventions.md        # file naming and folder rules
  extension-guide.md           # how to add or archive templates
templates/
  master/
    MASTER_SESSION_TEMPLATE.md # universal session bootstrap
  workflows/
    BUGFIX_TEMPLATE.md         # debugging and root cause
    FEATURE_BUILD_TEMPLATE.md  # adding new functionality
    REFACTOR_TEMPLATE.md       # improving code without changing behavior
    CI_AUTOMATION_TEMPLATE.md  # pipelines, scripts, release flows
    DOCS_HARDENING_TEMPLATE.md # README, setup, onboarding clarity
    REPO_REVIEW_TEMPLATE.md    # architecture and quality audit
    RESEARCH_COMPARISON_TEMPLATE.md  # evaluating options
    MIGRATION_TEMPLATE.md        # DB migrations, framework upgrades
    SECURITY_REVIEW_TEMPLATE.md  # OWASP checklist, dependency audit
    PERFORMANCE_TEMPLATE.md      # profiling and optimization
  builders/
    TEMPLATE_BUILDER.md        # create a new template from scratch
    WORK_SPECIFIC_TEMPLATE_PROMPT.md  # meta-prompt for the model
  custom/
    REPO_CREDIBILITY_TEMPLATE.md     # interview prep, OSS polish
    CLI_TOOL_BUILD_TEMPLATE.md       # CLI tool development
    AI_WORKFLOW_BUILD_TEMPLATE.md    # agent workflow design
    OSS_POSITIONING_TEMPLATE.md      # public repo framing
    CI_HARDENING_TEMPLATE.md         # tightening quality gates
    PROOF_OF_METHOD_TEMPLATE.md      # proving engineering discipline
    DEPLOY_TEMPLATE.md               # Fly.io/Vercel deployment sessions
    HACKATHON_SPRINT_TEMPLATE.md     # time-boxed delivery with deadline
    TEST_COVERAGE_TEMPLATE.md        # targeted coverage improvement
examples/
  example_bugfix_session.md          # SQLite WAL deadlock diagnosis
  example_feature_session.md         # webhook subscription endpoints
  example_repo_review.md             # developer tool credibility audit
  example_work_specific_template_request.md  # blockchain ingestion template
  example_migration_session.md         # SQLAlchemy 1.4 → 2.0 upgrade
  example_security_review.md           # pre-public API security audit
  example_deploy_session.md            # Fly.io + Litestream first deploy
  example_master_template_session.md   # multi-step bulk operations
  example_hackathon_sprint.md          # 72-hour EVE Frontier sprint
  example_test_coverage_grind.md       # 70% → 80% coverage push
```

## Quick start

1. Pick the closest template from `templates/workflows/` or `templates/custom/`.
2. Copy it into your session notes or directly into the coding-agent prompt.
3. Fill in only the fields that matter.
4. Point the model at likely relevant files.
5. Require structured output:
   - summary
   - files changed
   - risks / assumptions
   - validation performed
6. If the task repeats, create a specialized template in `templates/custom/`.

## Best use pattern

Use the master template for broad sessions.  
Use workflow templates for common engineering work.  
Use custom templates for higher-value repeated work.  
Use builder templates when none of the existing templates fit.

## Recommended workflow

- define the objective
- define scope
- define constraints
- define likely files
- define validation
- define required output

That is the difference between “asking AI for help” and running a disciplined engineering session.

## Most important files

Start with:
- `templates/master/MASTER_SESSION_TEMPLATE.md`
- `templates/workflows/BUGFIX_TEMPLATE.md`
- `templates/workflows/FEATURE_BUILD_TEMPLATE.md`
- `templates/builders/TEMPLATE_BUILDER.md`

## Why this exists

Most coding-agent failures start before the model writes any code.  
They start with weak session setup.

This repo is a lightweight operating system for preventing that.
