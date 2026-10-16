---
name: turtle-backlog
description: Use this when converting IDEATE output or an explicitly approved Turtle design into a structured backlog, or when updating docs/backlog.md with new items. Do not use for planning, implementation, debugging, or testing.
---

## Always read
- agents.md
- architecture.md
- repo_map.md

## When to use
Use after IDEATE to convert ideas into a persistent backlog, after design approval to persist the approved feature, or when updating the backlog with new items.

## Inputs required
- IDEATE output or `docs/design/<feature_slug>/approved-design.md`
- existing `docs/backlog.md` if present

At least one feature source must exist. When an approved design is the source, require it to state that the design is approved for planning, read it, and use its approved goal and scope without introducing unapproved design work. If its approval is missing or ambiguous, stop and route the feature to `/turtle-design-checkpoint`.

Also read `docs/system/design-workflow.md` when an approved design is the feature source.

## Output expected
- updated `docs/backlog.md`
- markdown task list
- grouped sections
- preserved status on existing items
- only new items appended when needed

## Before writing
- check if docs/backlog.md exists
- if it exists, read it and preserve its structure while extending it incrementally
- prefer editing existing sections over creating new ones; avoid duplication
- if it does not exist, create it (and create docs/ if missing)

## Rules
- this phase is for organizing ideas into a backlog; do NOT plan implementation details
- do NOT invent features or items not present in IDEATE output, an approved design, or the existing backlog
- preserve status of existing items; never flip [x] to [ ]
- avoid duplicates; merge with existing items when equivalent
- when the source is an approved design, normalize and compare the proposed item against existing feature wording and the feature slug; reuse an equivalent item instead of appending a duplicate
- keep items concise and actionable (one line each)
- use snake_case wording for file-like references when applicable
- treat approved design content, code, and the existing backlog as source of truth over assumptions

Convert the feature source into a structured product backlog.
- base all items on IDEATE output, an approved design, and the existing backlog; do not introduce new scope

## Backlog Structure
Group items into:
- Features
- Architecture Improvements
- Client Improvements

## Formatting
Formatting rules

Use markdown task list format for every item:

- [ ] Not started
- [x] Completed

- keep descriptions short, verb-first (e.g., "add search filter", "refactor auth service")

Preserve the status of existing items when updating the backlog.

If an item already exists in docs/backlog.md:
- keep its existing status
- do not duplicate it
- return the exact existing item so a controller can pass it to `/turtle-plan`

If the item is new:
- add it as unchecked (- [ ])

Optional:
- Within each section, list the most practical / highest-leverage items first

## Output
Output the backlog as markdown.

Save to docs/backlog.md.
If docs/backlog.md does not exist, create it.
If docs/ does not exist, create it.

Do not remove completed items.
Do not change checked items back to unchecked.
Only append new items if they do not already exist.

## Notes
- prioritize highest-leverage items near the top of each section
- call out any assumptions explicitly if the feature source is ambiguous
- design approval authorizes backlog creation only; do not create a plan or modify production code
