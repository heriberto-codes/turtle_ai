---
name: turtle-document
description: Use this when creating or updating docs/features/<feature_slug>.md to document a completed feature, including summary, decisions, architecture impact, tests, and follow-ups. Typically used after implementation, verification, and backlog updates are complete. Do not use for planning, implementation, debugging, or testing.
---

## Always read
- agents.md
- architecture.md
- repo_map.md

## When to use
- after the feature is fully complete and verified
- after backlog_update and before commit

## Inputs
- feature_slug
- docs/plans/<feature_slug>_plan.md
- docs/backlog.md
- modified files (git diff)
- test results and coverage impact
- deployment notes (if any)

## Output
- create or update: docs/features/<feature_slug>.md
- write the full markdown directly to docs/features/<feature_slug>.md
- a concise, factual feature record including summary, decisions, architecture impact, tests, and follow-ups
- do not only print the markdown to the terminal

## Plan-Driven Guardrail
Before documenting, verify that the feature is fully completed in the Plan-Driven State System.

Required checks:

1. Locate the related plan in `docs/plans/`.
2. Confirm every actionable plan step is checked `[x]`.
3. Confirm there are no remaining unchecked actionable steps `[ ]`.
4. Confirm the feature passed the required validation gates:
   - EXECUTE
   - VERIFY
   - ENGINEER CHECKPOINT
   - TEST
   - DEBUG, only if debugging was needed
5. Confirm the related backlog item is already marked `[x]` in `docs/backlog.md`.

If these checks do not pass, stop and report:

```text
Documentation blocked. Feature is not fully completed in the Plan-Driven State System.
```

## Before writing
- check if docs/features/<feature_slug>.md exists
- if it exists, read it and preserve structure while extending it incrementally
- prefer updating existing sections over creating new ones; avoid duplication
- if it does not exist, create docs/features/ if missing, then create the file

## Task
Create or update the feature record for <feature_slug> based on actual implementation and repository changes.

## Sections
Include the following sections (only if applicable):
- Summary
- Architecture Impact
- Decisions
- API Changes
- Database Changes
- Test Impact
- Deployment Notes
- Follow-ups / Accepted Risks

## Rules
- document only what was actually implemented (based on diffs and tests)
- be concise and structured; avoid repetition
- do NOT restate the full plan; summarize outcomes only
- do NOT invent changes or future work; label follow-ups explicitly
- prefer bullets over long paragraphs
- preserve existing headings and formatting when updating
- treat code and tests as source of truth over documentation

## Output Format
- create or update the file: docs/features/<feature_slug>.md
- write the full markdown directly to that file
- do NOT only print the markdown to the terminal
- do NOT ask the user to manually copy/paste the documentation
- return the markdown as plain output only if file writing fails

## Constraints
- only modify docs/features/<feature_slug>.md
- if the file exists, update it in place
- if the file does not exist, create it
- do NOT modify any files other than docs/features/<feature_slug>.md
- do NOT ask the user to manually copy/paste
- do NOT return the markdown as plain output unless file writing fails
- do NOT include git commands or extra commentary
- if context is incomplete, state assumptions briefly under Follow-ups / Accepted Risks