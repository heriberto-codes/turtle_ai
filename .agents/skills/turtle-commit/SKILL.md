---
name: turtle-commit
description: Use this when generating a final, concise git commit message for a completed feature, based on plans, feature docs, backlog updates, and modified files. Typically used after implementation, verification, and documentation are complete. Do not use for planning, implementation, debugging, or modifying code.
---

## Always read
- agents.md
- architecture.md
- repo_map.md

## When to use
Use after the feature is complete and documented, when preparing the final git commit message.

## Inputs required
- feature slug
- `docs/plans/<feature_slug>_plan.md`
- `docs/features/<feature_slug>.md`
- `docs/backlog.md`
- modified source files

## Output expected
- commit message in `type(scope): short summary` format
- short commit body
- list of main files changed
- no git commands executed

## Before writing
- verify all required docs exist; if any are missing, proceed with available context and note assumptions
- read plan, feature doc, backlog, and inspect modified files
- do not modify any files; this step is read-only

## Rules
- this phase generates a commit message only; do NOT modify code or run git commands
- keep the subject line concise (≤ 72 characters)
- use imperative mood (e.g., "add", "fix", "refactor")
- choose an appropriate type: feat | fix | refactor | chore | docs | test | perf | build | ci
- scope should reflect the primary module or domain affected
- do NOT invent changes; base all statements on actual diffs and docs
- prefer the smallest accurate scope and summary
- avoid duplication between subject and body

## Task
Prepare a git commit message for the completed feature based on repository changes and documentation.

## Inputs
- feature_slug
- docs/plans/<feature_slug>_plan.md
- docs/features/<feature_slug>.md
- docs/backlog.md
- modified source files (git diff)

## Output
Return only the following sections:

1) Subject
`type(scope): short summary`

2) Body
- what was implemented
- key architectural decisions
- notable files or modules affected

3) Files Changed
- list of main files (paths)

## Constraints
- do NOT include git commands
- do NOT include extra commentary
- keep output minimal and structured
- if context is incomplete, state assumptions briefly in the body