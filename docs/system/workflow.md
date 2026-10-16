# Turtle AI Workflow Reference

This document contains the full Turtle AI workflow. The README gives the short version; this file is the operational reference.

## Workflow Overview

```text
FOUNDATION -> DISCOVERY -> OPTIONAL DESIGN -> PLANNING -> IMPLEMENTATION LOOP -> HARDENING -> FINALIZATION
```

## Foundation

Run these in order at initial setup, then refresh the affected documents when repository rules, architecture, or structure materially change. In an empty repository, record unknowns and refresh after initial scaffolding.

| Order | Skill | Purpose |
| --- | --- | --- |
| 1 | `$turtle-agents` | Define project rules and safety constraints so AI follows repository conventions. |
| 2 | `$turtle-architecture` | Capture the system blueprint and architectural patterns to preserve. |
| 3 | `$turtle-repo-map` | Create a high-level navigation map of important files, protected paths, and critical modules. |

This order is also the bootstrap contract for a new project:

- `$turtle-agents` inspects the repository directly and creates `AGENTS.md`; missing `architecture.md` and `repo_map.md` do not block it.
- `$turtle-architecture` requires `AGENTS.md` but may run before `repo_map.md` exists.
- `$turtle-repo-map` runs after both `AGENTS.md` and `architecture.md` exist.

`AGENTS.md` is the canonical repository-instructions filename. Turtle skills temporarily accept lowercase `agents.md` only when the canonical file is absent. `$turtle-agents` owns migration: it renames a legacy-only file while preserving its valid content, and it stops for user reconciliation when both variants exist.

Begin a fresh Codex session after creating or changing repository instructions. When adopting Turtle in another project, copy the skill folders and all four shared system documents, then generate that project's foundation context; see the [README setup guide](https://github.com/heriberto-codes/turtle_ai#install-turtle-in-your-project).

## Discovery

Use discovery when direction, scope, or repository context is unclear.

| Skill | When useful | Purpose |
| --- | --- | --- |
| `$turtle-analyze` | Repository or module context is unfamiliar or stale. | Build a working mental model of the repository. |
| `$turtle-ideate` | Feature direction needs exploration. | Generate grounded feature ideas. |
| `$turtle-backlog` | Ideas are ready to persist, or a design has explicit approval. | Persist and prioritize features in `docs/backlog.md`. |

Discovery is not a mandatory three-command sequence. Without design exploration, turn ideation output into a backlog item and then plan it, or plan an existing unchecked item directly. With design exploration, use the design controller first; after approval it creates or reuses the backlog item and creates the plan, in separate invocations.

## Optional Design

For user-facing work, complete the optional design phase before planning. For work without meaningful design decisions, proceed directly from discovery.

Use `$turtle-design-next` as the state-aware controller. Run it repeatedly; each invocation performs exactly one safe transition and then stops.

```text
AUDIT -> SECTION CONFIRMATION -> SECTION MOCKUPS AND SELECTIONS -> ASSEMBLY -> CHECKPOINT -> BACKLOG -> PLAN
```

The controller pauses for the user to:

- confirm the audit's proposed ordered section list
- select, combine, or revise each section's mockup directions
- explicitly approve the complete assembled design

It stores controller state in `docs/design/<feature_slug>/workflow-state.md`. It may finish by creating the matching backlog item and implementation plan, but it never runs `$turtle-execute` or changes production code.

The specialist commands remain available for manual use:

| Skill | Purpose |
| --- | --- |
| `$turtle-design-audit` | Inspect the current interface and propose design opportunities and page sections. |
| `$turtle-design-mockup` | Create or revise isolated responsive mockup directions. |
| `$turtle-design-checkpoint` | Review the assembled design and capture explicit approval for planning. |

See `docs/system/design-workflow.md` for artifact ownership, state values, and transition rules.

## Planning

| Order | Skill | Purpose |
| --- | --- | --- |
| 1 | `$turtle-plan` | Convert a selected backlog item into `docs/plans/<feature_slug>_plan.md`. |

When a matching design workspace exists, planning requires its approved design. `$turtle-design-next` can invoke the same planning contract after approval and backlog creation, then stops before implementation.

## Implementation Loop

Each plan step moves through this loop:

```text
EXECUTE -> VERIFY -> ENGINEER CHECKPOINT -> TEST -> PLAN STEP UPDATE
```

| Step | Skill | Gate Type | Purpose |
| --- | --- | --- | --- |
| 1 | `$turtle-execute` | Build | Implement only the first unchecked plan step. |
| 2 | `$turtle-verify` | Quality | Review correctness, scope, and plan alignment. |
| 3 | `$turtle-engineer-checkpoint` | Comprehension | Confirm the engineer can explain the change. |
| 4 | `$turtle-test` | Validation | Review required tests, then implement the smallest correct test set. |
| As needed | `$turtle-debug` | Recovery | Diagnose failures, then apply the minimal correct fix when needed. |
| 5 | `$turtle-plan-step-update` | State Control | Mark the step complete only after all checks pass. |

This loop runs repeatedly until all plan steps are complete.

DEBUG is a recovery path only when needed. Failed reviews, checkpoints, or tests stop advancement; they do not authorize updating the checkbox. Diagnose unclear causes, route code or plan-alignment fixes through EXECUTE and test fixes through TEST, and repeat the affected validation gates before PLAN STEP UPDATE. After code changes, return through VERIFY, ENGINEER CHECKPOINT, and TEST. A checkpoint misunderstanding follows its bounded recovery conversation; it does not automatically require a code fix.

## Test Modes

`$turtle-test` has two modes.

### Mode A: Review

- Identify the smallest correct set of tests required for the active plan step.
- Determine whether existing tests already cover the behavior.
- Do not write or modify tests in this mode.

### Mode B: Implement

- Write or update only the tests identified in review mode.
- Keep test scope limited to the active plan step.
- Follow existing test patterns and conventions.

Mode A runs before Mode B. If Mode A identifies tests to add or update, run `$turtle-test` again and it will continue in the correct mode.

## Debug Modes

`$turtle-debug` has two modes.

### Mode A: Diagnose

- Identify the root cause of the failure.
- Determine whether the issue is in code, plan alignment, or tests.
- Do not apply fixes in this mode.

### Mode B: Apply Fix

- Apply only the minimal fix identified in diagnose mode.
- Fix the correct layer: code, plan alignment, or tests.
- Do not introduce unrelated scope.

Use `$turtle-debug` when a workflow or runtime/manual validation failure needs diagnosis. Follow its selected repair route; diagnosis or a fix alone is not evidence that the step is complete.

## Hardening

| Order | Skill | Purpose |
| --- | --- | --- |
| 1 | `$turtle-security` | Review new or changed behavior for security risks. |
| 2 | `$turtle-performance` | Review performance-sensitive paths when relevant. |

## Finalization

| Order | Skill | Purpose |
| --- | --- | --- |
| 1 | `$turtle-backlog-update` | Mark the feature complete in `docs/backlog.md`. |
| 2 | `$turtle-document` | Capture completed feature decisions and outcomes in `docs/features/`. |
| 3 | `$turtle-commit` | Prepare the final commit message. |

`$turtle-commit` prepares text; it does not automatically stage, commit, push, open a pull request, or publish a release. Git and GitHub actions require a separate user request.

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
|       |-- turtle-design-next/
|       |   `-- SKILL.md
|       |-- turtle-design-audit/
|       |   `-- SKILL.md
|       |-- turtle-design-mockup/
|       |   `-- SKILL.md
|       |-- turtle-design-checkpoint/
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
|   |-- design/
|   |   `-- <feature_slug>/
|   |       |-- workflow-state.md
|   |       |-- audit.md
|   |       |-- design-spec.md
|   |       |-- checkpoint.md
|   |       |-- approved-design.md
|   |       `-- mockups/
|   |-- system/
|   |   |-- design-workflow.md
|   |   |-- workflow.md
|   |   |-- state.md
|   |   `-- rules.md
|   |-- backlog.md
|   |-- plans/
|   |   `-- <feature_slug>_plan.md
|   `-- features/
|       `-- <feature_slug>.md
|-- AGENTS.md
|-- architecture.md
`-- repo_map.md
```

| Path | Purpose |
| --- | --- |
| `AGENTS.md` | Concise repository guardrails loaded as Codex instructions. |
| `.agents/skills/` | Codex skills that execute the Turtle AI workflow. |
| `docs/analysis/` | Repository understanding and system insights. |
| `docs/design/` | Optional design evidence, mockups, approval, and controller state. |
| `docs/system/` | Shared workflow rules and system references. |
| `docs/backlog.md` | Feature backlog and prioritization. |
| `docs/plans/` | Active feature execution state. |
| `docs/features/` | Finalized feature records and outcomes. |
