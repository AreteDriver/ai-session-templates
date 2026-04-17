# Best Practices

## The Hard Truth

Most people waste coding agents because they start with:

> "Help me with this repo."

Too vague. Too open. Too expensive. The model will produce fluent, confident, wrong output and you will spend more time fixing it than you saved.

A better pattern:
- Define the work type
- Define the scope
- Define the constraints
- Define validation
- Define the output shape

That is how you get reliable leverage instead of expensive noise.

## Session Discipline

1. **Start with the smallest correct scope.** If you cannot describe the task in two sentences, it is too broad for one session. Break it down.

2. **Prefer reviewable diffs.** A 20-line change you can verify beats a 500-line change you have to trust. Small changes compound faster than big rewrites.

3. **Align with repo conventions before inventing new patterns.** Check how the codebase already does things. The model will happily introduce a new pattern that conflicts with the existing one if you do not tell it otherwise.

4. **Require assumptions to be stated explicitly.** If the model had to guess something — a config value, an API behavior, a business rule — it should say so. Silent assumptions are where bugs hide.

5. **Always define validation.** "Does it work?" is not validation. Name the tests to run, the lint checks, the manual steps. If you cannot define how to verify the output, you are not ready to request it.

6. **Ask for structured output.** Summary, files changed, risks, validation performed. Every time. This is not overhead; it is the minimum for catching mistakes before they propagate.

7. **Treat fluent output as untrusted.** The model writes confidently whether it is right or wrong. Trust tests and diffs, not prose. If the model says "this handles all edge cases," verify that claim. It probably does not.

8. **Favor maintainability over cleverness.** Clever solutions are expensive to debug. Clear, boring solutions that match the existing codebase are almost always the right choice.

## Anti-Patterns

### Vague Prompts
| Bad | Better |
|-----|--------|
| "Help me with this repo" | "Add rate limiting to the /api/search endpoint using the existing middleware pattern in src/middleware/" |
| "Clean this up" | "Remove dead code in src/utils/legacy.py — functions not imported anywhere" |
| "Refactor this" | "Extract the validation logic from create_user() into a separate validate_user_input() function, keeping the same behavior" |
| "Build the best version" | "Add --json output flag to the lint command, matching the format used by ruff's JSON output" |

### Scope Creep
- Do not mix big refactors into small fixes unless you planned for it
- Do not let the model "improve" code outside the requested scope
- If the model suggests additional changes, evaluate them separately

### Ignoring the Model's Limitations
- AI helps most when the bottleneck is translation, repetition, or synthesis
- AI helps least when the bottleneck is judgment, architecture, or undefined goals
- The model does not know your production environment, your users, or your team's priorities unless you tell it
- The model will confidently produce code that compiles but does not do what you need if the requirements are ambiguous

## When to Stop and Think

Stop prompting and think when:
- You are not sure what "done" looks like
- The task requires understanding user behavior or business context the model does not have
- You have iterated on the same problem three times without progress
- The model's output keeps being wrong in ways you cannot easily define

In those cases, the bottleneck is not the model. It is the problem definition. Fix that first.
