---
name: turtle-backlog
description: Use this when converting IDEATE output into a structured backlog or when updating docs/backlog.md with new items. Typically used after ideation or when refining product scope. Do not use for planning, implementation, debugging, or testing.
---

## Always read
- agents.md
- architecture.md
- repo_map.md

## When to use
Use after IDEATE to convert ideas into a persistent backlog, or when updating the backlog with new items.

## Inputs required
- IDEATE output
- existing `docs/backlog.md` if present

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
- do NOT invent features or items not present in IDEATE output or existing backlog
- preserve status of existing items; never flip [x] to [ ]
- avoid duplicates; merge with existing items when equivalent
- keep items concise and actionable (one line each)
- use snake_case wording for file-like references when applicable
- treat code and existing backlog as source of truth over assumptions

Convert the IDEATE results into a structured product backlog.
- base all items on IDEATE output and existing backlog; do not introduce new scope

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
- call out any assumptions explicitly if IDEATE output is ambiguous