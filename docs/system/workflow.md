# Turtle AI Workflow Reference

This document contains the full Turtle AI workflow. The README gives the short version; this file is the operational reference.

## Workflow Overview

```text
FOUNDATION -> DISCOVERY -> PLANNING -> IMPLEMENTATION LOOP -> HARDENING -> FINALIZATION
```

## Foundation

Run these once to establish static project context.

| Order | Skill | Purpose |
| --- | --- | --- |
| 1 | `/turtle-agents` | Define project rules and safety constraints so AI follows repository conventions. |
| 2 | `/turtle-architecture` | Capture the system blueprint and architectural patterns to preserve. |
| 3 | `/turtle-repo-map` | Create a high-level navigation map of important files, protected paths, and critical modules. |

## Discovery

Use discovery when direction, scope, or repository context is unclear.

| Order | Skill | Purpose |
| --- | --- | --- |
| 4 | `/turtle-ideate` | Generate or explore potential features. |
| 5 | `/turtle-backlog` | Persist and prioritize features in `docs/backlog.md`. |
| 6 | `/turtle-analyze` | Build a working mental model of the repository. |

## Planning

| Order | Skill | Purpose |
| --- | --- | --- |
| 7 | `/turtle-plan` | Convert a selected backlog item into `docs/plans/<feature_slug>_plan.md`. |

## Implementation Loop

Each plan step moves through this loop:

```text
EXECUTE -> VERIFY -> ENGINEER CHECKPOINT -> TEST -> DEBUG -> PLAN STEP UPDATE
```

| Step | Skill | Gate Type | Purpose |
| --- | --- | --- | --- |
| 8 | `/turtle-execute` | Build | Implement only the first unchecked plan step. |
| 9 | `/turtle-verify` | Quality | Review correctness, scope, and plan alignment. |
| 10 | `/turtle-engineer-checkpoint` | Comprehension | Confirm the engineer can explain the change. |
| 11 | `/turtle-test` | Validation | Review required tests, then implement the smallest correct test set. |
| 12 | `/turtle-debug` | Recovery | Diagnose failures, then apply the minimal correct fix when needed. |
| 13 | `/turtle-plan-step-update` | State Control | Mark the step complete only after all checks pass. |

This loop runs repeatedly until all plan steps are complete.

## Test Modes

`/turtle-test` has two modes.

### Mode A: Review

- Identify the smallest correct set of tests required for the active plan step.
- Determine whether existing tests already cover the behavior.
- Do not write or modify tests in this mode.

### Mode B: Implement

- Write or update only the tests identified in review mode.
- Keep test scope limited to the active plan step.
- Follow existing test patterns and conventions.

Mode A runs before Mode B. If Mode A identifies tests to add or update, run `/turtle-test` again and it will continue in the correct mode.

## Debug Modes

`/turtle-debug` has two modes.

### Mode A: Diagnose

- Identify the root cause of the failure.
- Determine whether the issue is in code, plan alignment, or tests.
- Do not apply fixes in this mode.

### Mode B: Apply Fix

- Apply only the minimal fix identified in diagnose mode.
- Fix the correct layer: code, plan alignment, or tests.
- Do not introduce unrelated scope.

Only run `/turtle-debug` when something fails in `EXECUTE`, `VERIFY`, `ENGINEER CHECKPOINT`, or `TEST`.

## Hardening

| Order | Skill | Purpose |
| --- | --- | --- |
| 14 | `/turtle-security` | Review new or changed behavior for security risks. |
| 15 | `/turtle-performance` | Review performance-sensitive paths when relevant. |

## Finalization

| Order | Skill | Purpose |
| --- | --- | --- |
| 16 | `/turtle-backlog-update` | Mark the feature complete in `docs/backlog.md`. |
| 17 | `/turtle-document` | Capture completed feature decisions and outcomes in `docs/features/`. |
| 18 | `/turtle-commit` | Prepare the final commit message. |

## Recommended Project Structure

This is the recommended structure after adopting Turtle AI. It is not necessarily the literal file tree of this repository.

```text
project-root
|-- .agents/
|   `-- skills/
|       |-- turtle-agents/
|       |   `-- SKILL.md
|       |-- turtle-architecture/
|       |   `-- SKILL.md
|       |-- turtle-repo-map/
|       |   `-- SKILL.md
|       |-- turtle-ideate/
|       |   `-- SKILL.md
|       |-- turtle-backlog/
|       |   `-- SKILL.md
|       |-- turtle-analyze/
|       |   `-- SKILL.md
|       |-- turtle-plan/
|       |   `-- SKILL.md
|       |-- turtle-execute/
|       |   `-- SKILL.md
|       |-- turtle-verify/
|       |   `-- SKILL.md
|       |-- turtle-engineer-checkpoint/
|       |   `-- SKILL.md
|       |-- turtle-test/
|       |   `-- SKILL.md
|       |-- turtle-debug/
|       |   `-- SKILL.md
|       |-- turtle-plan-step-update/
|       |   `-- SKILL.md
|       |-- turtle-security/
|       |   `-- SKILL.md
|       |-- turtle-performance/
|       |   `-- SKILL.md
|       |-- turtle-backlog-update/
|       |   `-- SKILL.md
|       |-- turtle-document/
|       |   `-- SKILL.md
|       `-- turtle-commit/
|           `-- SKILL.md
|-- docs/
|   |-- analysis/
|   |   `-- repo_analysis.md
|   |-- system/
|   |   |-- workflow.md
|   |   |-- state.md
|   |   `-- rules.md
|   |-- backlog.md
|   |-- plans/
|   |   `-- <feature_slug>_plan.md
|   `-- features/
|       `-- <feature_slug>.md
|-- agents.md
|-- architecture.md
`-- repo_map.md
```

| Path | Purpose |
| --- | --- |
| `.agents/skills/` | Codex skills that execute the Turtle AI workflow. |
| `docs/analysis/` | Repository understanding and system insights. |
| `docs/system/` | Shared workflow rules and system references. |
| `docs/backlog.md` | Feature backlog and prioritization. |
| `docs/plans/` | Active feature execution state. |
| `docs/features/` | Finalized feature records and outcomes. |
