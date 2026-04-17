---
version: "1.0.0"
updated: "2026-04-17"
---

# Feature Build Template

Use this when adding new functionality to an existing project. Captures the user need,
functional and non-functional requirements, and enforces alignment with existing patterns.

```md
# Feature Build Session

## Feature
[Name of feature]

## User / Operator Need
[Who needs this and why?]

## Desired Outcome
[What should exist by the end?]

## Functional Requirements
- [requirement]
- [requirement]

## Non-Functional Requirements
- performance: [expectation]
- security: [expectation]
- maintainability: [expectation]
- compatibility: [expectation]

## Existing Patterns to Follow
- [pattern / file / component]
- [pattern / file / component]

## Files Likely Relevant
- [path]
- [path]

## Constraints
- avoid unnecessary abstraction
- align with current architecture
- include tests/docs if appropriate

## Required Output
- implementation plan
- code changes
- docs updates if needed
- test/validation summary
```
