# Extension Guide

## When to Create a New Template

Create a new template when:
- The work pattern repeats (you have done this type of session more than twice)
- The work has distinct risks or validation needs that generic templates miss
- Existing templates require heavy editing to fit — more than half the sections need rewriting
- The domain has specific anti-patterns you want to guard against every time

Do not create a new template when:
- An existing template covers it with minor edits
- The work is a one-off that will not repeat
- The difference is cosmetic (wording preference, not structural)

## How to Add a New Template

### Step 1: Decide Where It Goes

| Folder | Criteria |
|--------|----------|
| `templates/workflows/` | Common engineering work that any developer does regularly (bug fix, feature build, refactor) |
| `templates/custom/` | Specialized or high-value work patterns specific to your operating style (repo credibility, proof of method, AI workflow) |
| `templates/builders/` | Meta-templates for generating new templates (rarely needs additions) |

### Step 2: Name It

Follow the naming convention: `UPPERCASE_SNAKE_CASE_TEMPLATE.md`

Good names:
- `API_INTEGRATION_TEMPLATE.md`
- `DATA_MIGRATION_TEMPLATE.md`
- `PACKAGE_PUBLISH_TEMPLATE.md`

Bad names:
- `my_template.md` (not descriptive)
- `NewThing.md` (wrong case, no suffix)
- `AWESOME_MEGA_TEMPLATE.md` (marketing, not engineering)

### Step 3: Write It

Every template needs these sections at minimum:
- **Objective**: what the session accomplishes
- **Scope**: in-scope and out-of-scope boundaries
- **Constraints**: rules the model must follow
- **Validation**: how to verify the output is correct
- **Required output**: what the model delivers
- **What to avoid**: specific anti-patterns for this work type

Optional but recommended:
- Relevant files section
- Risks / failure modes
- Examples

Use the Template Builder (`templates/builders/TEMPLATE_BUILDER.md`) if you want a guided process.

### Step 4: Format It

- Start with a 2-3 line description before the template code block
- Wrap the template in a markdown code block (triple backticks with `md` language hint)
- Keep it concise enough to fill out in under 5 minutes

### Step 5: Test It

Use the template in a real session before considering it final. A template that looks good but produces vague output needs tighter constraints or better structure.

## How to Improve Weak Templates

If a template produces vague or unhelpful output:

1. **Tighten scope** — is the objective specific enough?
2. **Add stronger constraints** — is the model doing things you do not want?
3. **Clarify validation** — does the template define what "correct" looks like?
4. **Define output format** — does the model know exactly what to deliver?
5. **Add anti-patterns** — are there specific mistakes to call out in a "what to avoid" section?

## How to Archive Templates

When a template is no longer useful:

1. Do not delete it silently — someone might be referencing it
2. Move it to an `_archived/` folder within the same directory, or delete it and note the removal in a commit message
3. If the template was replaced, note what replaced it

Criteria for archiving:
- Has not been used in 3+ months
- Superseded by a better template
- The work pattern it covers no longer exists
- It consistently produces poor results despite revision attempts

## Keeping the Library Clean

This repo gets worse if it becomes a dumping ground. Apply these rules:

- Every template should earn its place through repeated use
- If two templates overlap significantly, merge them or delete the weaker one
- Review the template library quarterly — archive what is not pulling its weight
- Do not preserve templates for sentimental reasons
- Quality over quantity: 10 sharp templates beat 30 vague ones
