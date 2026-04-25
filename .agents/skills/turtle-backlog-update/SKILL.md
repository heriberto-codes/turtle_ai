---
name: turtle-backlog-update
description: Use this only after a feature has fully completed the Plan-Driven State System and needs to be marked done in docs/backlog.md. This skill updates one matching backlog item from unchecked to checked. Do not use for creating backlog items, planning, implementation, debugging, testing, or summarizing the backlog.
---

## Always read
- agents.md
- architecture.md
- repo_map.md
- docs/backlog.md
- relevant completed plan file in `docs/plans/`

## When to use
Use only after a feature has fully completed the execution loop and the corresponding backlog item should be marked done.

This skill is the final completion marker for backlog status.

Expected workflow position:

```text
PLAN → EXECUTE → VERIFY → ENGINEER CHECKPOINT → TEST → DEBUG if needed → PLAN STEP UPDATE → BACKLOG UPDATE
```

Do not use this skill before the plan is complete.

## Inputs required
One of the following must be available:

- completed feature name as written in `docs/backlog.md`
- active completed plan file in `docs/plans/`
- feature slug that uniquely maps to a completed plan and backlog item

Required files:

- `docs/backlog.md`
- relevant `docs/plans/<feature_slug>_plan.md`

## Output expected
- updated `docs/backlog.md`
- exactly one matching item changed from `- [ ]` to `- [x]`
- no other backlog items modified
- no summarized backlog output

## Plan-Driven State System alignment
This skill must respect the Plan-Driven State System.

Before updating `docs/backlog.md`, verify that the related plan is complete.

A plan is complete only when:

- every implementation step in the plan is checked `[x]`
- there are no remaining unchecked actionable plan steps `[ ]`
- the feature has passed the required validation gates:
  - EXECUTE
  - VERIFY
  - ENGINEER CHECKPOINT
  - TEST
  - DEBUG, only if debugging was needed
- PLAN STEP UPDATE has already marked the final plan step complete

If the plan is incomplete, stop and report:

```text
Backlog update blocked. The related plan is not complete yet.
Run the remaining Plan-Driven State System steps before marking the backlog item done.
```

## Current plan detection
If the completed feature name is not provided directly:

1. Inspect `docs/plans/`.
2. Locate the completed plan that corresponds to the active feature.
3. Prefer the plan file referenced by recent workflow context, if available.
4. Otherwise infer the feature name from:
   - the plan title
   - the feature slug
   - the plan filename
   - explicit feature metadata inside the plan
5. Match the inferred feature name to `docs/backlog.md`.

If no completed plan can be found, stop and report:

```text
Missing completed plan. Cannot safely update docs/backlog.md.
```

If more than one completed plan could match, stop and ask for clarification.

## Before writing
- verify `docs/backlog.md` exists; if missing, stop and report the error
- verify the relevant plan file exists; if missing, stop and report the error
- read `docs/backlog.md` and preserve its structure exactly
- read the related plan and confirm it is complete
- do not create or restructure backlog sections
- do not create a new backlog file
- do not create new backlog items

## Task
Update `docs/backlog.md` to reflect completed work by marking the matching backlog item as done.

Input:

- Feature name from backlog, completed plan file, or feature slug

## Matching rules
- locate the matching backlog item by exact text match first
- if no exact match exists, choose the closest unique match only when the match is obvious
- if multiple matches are found, stop and ask for clarification
- if no safe unique match exists, stop and report that no matching backlog item could be found
- do not guess from the backlog list alone
- do not mark an item complete based only on similar wording unless the match is unique and clearly tied to the completed plan

## Update rules
- change status from `- [ ]` to `- [x]` only for the matched item
- modify exactly one backlog item
- do NOT modify any other items
- do NOT reorder sections or items
- do NOT remove completed items
- do NOT rename backlog items
- do NOT rewrite headings
- do NOT alter indentation
- preserve formatting, headings, and spacing
- treat the existing backlog as the source of truth

## Stop conditions
Stop without editing if any of the following are true:

- `docs/backlog.md` is missing
- the relevant plan file is missing
- no completed feature name, feature slug, or completed plan can be identified
- the related plan still has unchecked actionable steps `[ ]`
- required validation gates are missing or incomplete
- no matching backlog item is found
- multiple possible backlog matches are found
- the matching backlog item is already checked `[x]`

## Response rules
- do not summarize the backlog
- do not output only a list of backlog items
- do not provide unrelated recommendations
- if editing the file directly is available, update `docs/backlog.md` directly instead of only printing the updated markdown
- include only the minimal change: one status flip from `- [ ]` to `- [x]`

Return the updated markdown for `docs/backlog.md`.