# Bug Fix Template

Use this when debugging a known issue. Forces structured reproduction, root cause isolation,
and validation before and after the fix. Keeps the change minimal and targeted.

```md
# Bug Fix Session

## Problem
[Describe the bug in plain language.]

## Expected Behavior
[What should happen?]

## Actual Behavior
[What is happening instead?]

## Reproduction
1. [step]
2. [step]
3. [step]

## Suspected Area
- [file/module]
- [service/component]

## Severity
[low / medium / high / production blocking]

## Constraints
- Fix root cause, not cosmetic symptom
- Prefer smallest safe change
- Preserve current behavior outside the bug
- Add or update tests if possible

## Required Output
- root cause
- fix
- impacted files
- test coverage / validation
- residual risk
```
