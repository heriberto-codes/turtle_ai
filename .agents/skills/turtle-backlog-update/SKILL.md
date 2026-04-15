---
name: turtle-backlog-update
description: Use this when marking a completed feature as done in docs/backlog.md by updating its status from unchecked to checked. Typically used after successful implementation and validation of a feature. Do not use for creating backlog items, planning, implementation, debugging, or testing.
---

## Always read
- agents.md
- architecture.md
- repo_map.md

## When to use
Use after the feature is complete to mark the corresponding backlog item as done.

## Inputs required
- completed feature name as written in backlog
- existing `docs/backlog.md`

## Output expected
- updated `docs/backlog.md`
- matching item changed from `- [ ]` to `- [x]`
- no other backlog items modified

## Before writing
- verify docs/backlog.md exists; if missing, stop and report the error
- read docs/backlog.md and preserve its structure exactly
- do not create or restructure sections

## Task
Update docs/backlog.md to reflect completed work by marking the matching item as done.

Input:
- Feature name from backlog (exact text or closest exact match)

## Rules
- locate the matching backlog item by exact text match; if no exact match, choose the closest unique match
- if multiple matches are found, stop and ask for clarification
- change status from `- [ ]` to `- [x]` only for the matched item
- do NOT modify any other items
- do NOT reorder sections or items
- do NOT remove completed items
- preserve formatting, headings, and indentation
- do NOT create a new backlog or new items
- treat the existing backlog as source of truth

## Output
- return the full updated markdown for docs/backlog.md
- include only the minimal change (status flip) with no additional edits

Return the updated markdown for docs/backlog.md.