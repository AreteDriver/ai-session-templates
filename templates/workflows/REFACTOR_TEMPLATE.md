# Refactor Template

Use this when improving code structure without changing intended behavior. Explicitly locks
down what must not change and keeps the diff reviewable and well-justified.

```md
# Refactor Session

## Refactor Goal
[What needs improvement?]

## Reason
- readability
- maintainability
- duplication
- performance
- dead code removal
- architecture cleanup

## Behavior That Must Not Change
- [behavior]
- [behavior]

## Areas In Scope
- [file/module]
- [file/module]

## Areas Out of Scope
- [file/module]
- [file/module]

## Constraints
- preserve functionality
- keep diff reviewable
- explain tradeoffs
- do not mix major behavior changes into refactor work

## Validation
- existing tests must pass
- add tests only if needed to lock behavior
- mention any untouched risk areas
```
