# Turtle AI Repository Instructions

## Purpose

Turtle AI keeps AI-assisted development small, reviewable, and understood. Preserve its plan-driven state model and comprehension gate.

## Instruction hierarchy

- This file contains concise, always-active repository guardrails.
- `architecture.md` is the current system blueprint, and `repo_map.md` is the current navigation reference. Keep both aligned with tracked repository behavior.
- `.agents/skills/*/SKILL.md` is the source of truth for each skill's procedure.
- `docs/system/` explains shared workflow and state rules.
- Code and tracked artifacts define current implementation. Do not invent missing systems or claim planned behavior exists.
- When changing a shared workflow contract, update every affected skill and system document in the same change.

## Skill routing

- Use the smallest matching skill; do not use a later phase to bypass an earlier gate.
- Foundation order for a new project is `turtle-agents` -> `turtle-architecture` -> `turtle-repo-map`.
- An approved design may authorize backlog creation and planning, but never production implementation.
- Respect skill output boundaries and stop conditions.

## Plan-driven invariants

- The first unchecked item in `docs/plans/<feature_slug>_plan.md` is active.
- Follow `EXECUTE -> VERIFY -> ENGINEER CHECKPOINT -> TEST -> DEBUG if needed -> PLAN STEP UPDATE`.
- Keep the active step unchecked throughout that loop.
- Only `turtle-plan-step-update` may change it from `[ ]` to `[x]` after every gate passes.
- Do not expand scope, skip stages, or combine phases.

## Repository guardrails

- Inspect existing files and preserve valid content.
- Keep changes minimal, in scope, and consistent with existing patterns.
- Never expose secrets, credentials, tokens, personal data, or `.env` contents.
- Do not modify generated output, dependencies, migrations, or destructive state without explicit authorization.
- Use snake_case for generated project artifacts unless a platform contract requires another name. `AGENTS.md` is an explicit uppercase exception.
- Put task-specific procedure in `SKILL.md` and explanatory detail in `docs/system/`.
