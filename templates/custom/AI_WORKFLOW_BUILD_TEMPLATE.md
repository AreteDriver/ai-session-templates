---
version: "1.0.0"
updated: "2026-04-17"
---

# AI Workflow Build Template

For designing and building agent workflows, multi-step LLM pipelines, or any system where AI models are execution components. Focuses on the parts most people skip: cost controls, failure modes, context management, and governance boundaries.

```md
# AI Workflow Build Session

## Workflow Purpose
[One sentence: what does this workflow accomplish end-to-end?]

## Why This Needs a Workflow (Not a Single Prompt)
[Explain why a single model call is insufficient. What makes this multi-step?]
Examples:
- sequential refinement (draft -> review -> finalize)
- fan-out / fan-in (parallel analysis, merged output)
- conditional routing (different paths based on intermediate results)
- human-in-the-loop checkpoints
- tool use with external data

## Agent Roles

Define each agent or step in the workflow. Be explicit about boundaries.

| Role | Model / Tool | Input | Output | Decision Authority |
|------|-------------|-------|--------|-------------------|
| [role name] | [model/tool] | [what it receives] | [what it produces] | [what it can decide vs. escalate] |
| [role name] | [model/tool] | [what it receives] | [what it produces] | [what it can decide vs. escalate] |

### Role Boundaries
- What can each agent do autonomously?
- What requires escalation or human approval?
- Which agents can invoke tools? Which tools?
- Are any agents allowed to modify their own instructions? (Answer should usually be no.)

## Context Management

### What Context Each Step Receives
- [step]: [specific context — not "everything"]
- [step]: [specific context]

### Context Size Controls
- Maximum input tokens per step: [number]
- Summarization strategy for long context: [truncate / summarize / chunk]
- What gets carried forward between steps vs. dropped?

### State Management
- Where is intermediate state stored? [memory / file / database / none]
- How is state passed between steps? [explicit handoff / shared store / message queue]
- What happens to state on failure? [preserved / rolled back / lost]

## Cost Controls

### Per-Step Budget
| Step | Model | Estimated Input Tokens | Estimated Output Tokens | Max Cost |
|------|-------|----------------------|------------------------|----------|
| [step] | [model] | [tokens] | [tokens] | [$X.XX] |

### Workflow-Level Controls
- Maximum total cost per run: [$X.XX]
- Maximum retry budget: [N retries or $X.XX]
- Cost circuit breaker: [what triggers abort]
- Model routing: [when to use cheaper model vs. expensive one]

### Cost Monitoring
- How is spend tracked? [logging / dashboard / alerts]
- Who gets notified on budget breach?

## Governance Constraints

### What the Workflow Must Not Do
- [boundary — e.g., must not modify production data without approval]
- [boundary — e.g., must not make external API calls without logging]
- [boundary — e.g., must not generate content that bypasses review]

### Audit Trail
- What gets logged? [inputs, outputs, decisions, costs, timestamps]
- Where are logs stored?
- Is the workflow reproducible from logs?

### Human Checkpoints
- Where in the workflow must a human approve before proceeding?
- What information does the human see at each checkpoint?
- What happens if the human rejects?

## Failure Modes

### Anticipated Failures
| Failure | Cause | Detection | Recovery |
|---------|-------|-----------|----------|
| Bad context | insufficient or stale input | output quality check | retry with better context or abort |
| Cost blowup | unbounded retries or large input | budget monitor | circuit breaker, abort |
| Infinite loop | agent re-invoking itself | step counter / timeout | hard limit, abort |
| Brittle routing | model makes wrong routing decision | output validation | fallback path or human escalation |
| Silent wrong answer | model confidently produces incorrect output | downstream validation / tests | require structured output with evidence |
| Partial failure | one step succeeds, next fails | step-level status tracking | resume from checkpoint or rollback |

### Fallback Behavior
- What happens when the primary model is unavailable?
- What happens when a tool call fails?
- Is there a degraded-but-functional mode?

## Output Validation

### Per-Step Validation
- [step]: [how output is validated before passing to next step]
- [step]: [how output is validated]

### Final Output Validation
- Schema or format check: [JSON schema / regex / type check]
- Content quality check: [automated scoring / human review / both]
- Acceptance criteria: [what makes the output "done"]

### What "Bad Output" Looks Like
- [specific failure pattern to detect]
- [specific failure pattern to detect]

## Implementation

### Stack
- Orchestration: [LangChain / custom / Animus / direct API calls]
- Models: [which models for which steps]
- Tools: [external APIs, databases, file systems]
- Infrastructure: [where this runs — local / cloud / CI]

### Required Files
- [path/file — what it does]
- [path/file — what it does]

## Scope

### In scope
- [item]
- [item]

### Out of scope
- [item — and why]
- [item — and why]

## Required Output
1. Workflow design (diagram or structured description of steps, inputs, outputs)
2. Implementation (code, configs, schemas)
3. Cost estimate (per-run and monthly at expected volume)
4. Failure mode coverage (which failures are handled, which are accepted risks)
5. Validation approach (how you know the workflow produces correct output)
6. Governance checklist (logging, human checkpoints, boundaries confirmed)

## What to Avoid
- vague agent role definitions ("the agent figures out what to do")
- unbounded context passing (dumping everything into every step)
- no cost controls ("we will optimize later")
- no failure handling ("the model is good enough")
- orchestration complexity that exceeds the value of the workflow
- treating the workflow as a demo instead of a production system
```
