# Overview

## Why This Exists

Most bad AI-assisted engineering work starts before the model writes a single line of code. It starts with bad setup: vague tasking, no scope, no constraints, no validation criteria, no awareness of the existing codebase.

These templates fix the setup problem.

## Core Philosophy

**Structure before freedom.** Give the model a stable operating frame before it writes code. Define the objective, scope, constraints, relevant files, validation requirements, and output format up front. That dramatically improves result quality and eliminates the most common failure mode: fluent garbage.

**Constrain scope aggressively.** The biggest mistake people make with coding agents is treating them like a smarter Stack Overflow. Power-user behavior looks different: you box the model into the repo's existing structure, you define what is in scope and what is not, you make the model work against the codebase rather than against your imagination.

**Trust tests over fluent output.** AI-generated code reads well. That is the trap. Fluent output is untrusted until validated. Tests, diffs, linters, and CI are the verification layer. If you do not build those gates, AI-assisted development turns into speed without control.

**Make the model work against the repo, not in the abstract.** Point at specific files, name specific patterns, describe specific constraints. The model is most useful when the bottleneck is translation, repetition, or synthesis. It is least useful when the bottleneck is judgment, architecture, or undefined goals.

**Compress iteration cycles, not replace engineering judgment.** AI helps most when you keep system design and acceptance criteria in human hands while letting the model accelerate the translation from intent to implementation. The real skill is not prompting. The real skill is knowing when to use the model, how tightly to scope it, and how to create a workflow where bad output gets caught cheaply.

## What This Repo Is

A session-setup system. Before you start working with a coding agent, you pick the closest template, fill in the fields that matter, and point the model at the relevant code. The result is a structured, bounded session instead of an open-ended conversation.

## What This Repo Is Not

- A generic prompt collection
- A replacement for engineering judgment
- A way to avoid understanding your own codebase
- A hype project about AI capabilities

## Design Principles

1. Every template must be copy-paste ready and usable without extra explanation
2. Templates enforce objective, scope, constraints, validation, and output format
3. Prefer small, reviewable changes over ambitious rewrites
4. Fit existing repo patterns before inventing new ones
5. Require assumptions to be stated explicitly
6. Optimize for real engineering work, not generic chat
