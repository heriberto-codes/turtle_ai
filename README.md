[![wakatime](https://wakatime.com/badge/user/3a78d911-a08a-40c2-bf2c-782c4d20eb23/project/083c1137-37af-417a-8f7d-632880b2dc31.svg)](https://wakatime.com/badge/user/3a78d911-a08a-40c2-bf2c-782c4d20eb23/project/083c1137-37af-417a-8f7d-632880b2dc31)

# Turtle AI

**Version:** v1.0.0

> Build with AI without losing understanding.

Turtle AI is an AI coding workflow where progress is gated by engineer comprehension, not just code generation. It is designed to preserve engineer understanding, codebase context, and long-term ownership while still using AI to move faster.

Most AI coding tools optimize for speed. Turtle AI optimizes for control.

## Why It Exists

The common failure mode of AI-assisted development is that the engineer gradually loses real understanding of their own codebase.

Turtle AI prevents that by keeping changes small, reviewable, and tied to a plan. The engineer remains the source of truth, while AI acts as a constrained implementation partner.

## Core Loop

Every implementation step moves through the same controlled loop:

```text
EXECUTE -> VERIFY -> ENGINEER CHECKPOINT -> TEST -> DEBUG -> PLAN STEP UPDATE
```

At the center of the loop is `ENGINEER CHECKPOINT`.

Before work can move forward, the engineer must be able to explain:

- what was implemented
- why it works
- how it fits into the system

That comprehension gate is the core distinction of Turtle AI.

## Quick Start

1. Open this repo in Codex.
2. Ensure `.agents/skills/` exists.
3. Run the foundation skills once:
   - `/turtle-agents`
   - `/turtle-architecture`
   - `/turtle-repo-map`
4. Optionally run discovery:
   - `/turtle-ideate`
   - `/turtle-backlog`
   - `/turtle-analyze`
5. Pick a feature from `docs/backlog.md`.
6. Create a plan:
   - `/turtle-plan`
7. Run the implementation loop until complete:
   - `/turtle-execute`
   - `/turtle-verify`
   - `/turtle-engineer-checkpoint`
   - `/turtle-test`
   - `/turtle-debug` if needed
   - `/turtle-plan-step-update`
8. Finalize the feature:
   - `/turtle-security`
   - `/turtle-performance`
   - `/turtle-backlog-update`
   - `/turtle-document`
   - `/turtle-commit`

The active step is always the first unchecked item in `docs/plans/<feature_slug>_plan.md`.

## What's Included

![What's Inside Turtle AI](./assets/turtleAI.png)

Turtle AI includes repo-scoped Codex skills in `.agents/skills/`.

| Category | Skills |
| --- | --- |
| Foundation | `turtle-agents`, `turtle-architecture`, `turtle-repo-map` |
| Discovery | `turtle-ideate`, `turtle-backlog`, `turtle-analyze` |
| Planning | `turtle-plan` |
| Implementation | `turtle-execute`, `turtle-verify`, `turtle-engineer-checkpoint`, `turtle-test`, `turtle-debug`, `turtle-plan-step-update` |
| Hardening | `turtle-security`, `turtle-performance` |
| Finalization | `turtle-backlog-update`, `turtle-document`, `turtle-commit` |

## Codex Skills Support

Turtle AI runs as a repo-scoped Codex skill set located in:

```text
.agents/skills/
```

Once the repository is opened, Codex discovers these skills automatically. Commands such as `/turtle-plan`, `/turtle-execute`, and `/turtle-verify` become available from the Codex skill system.

Turtle AI requires `.agents/skills/`. If that directory is missing, Turtle commands will not be available.

## Documentation

The README is the entry point. The detailed workflow reference lives in the system docs:

- [Workflow Reference](docs/system/workflow.md)
- [State System](docs/system/state.md)
- [Global Rules](docs/system/rules.md)
