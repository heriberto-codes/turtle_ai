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
- a concise, factual feature record including summary, decisions, architecture impact, tests, and follow-ups

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
Return only the full markdown for docs/features/<feature_slug>.md

## Constraints
- do NOT modify any files other than docs/features/<feature_slug>.md
- do NOT include git commands or extra commentary
- if context is incomplete, state assumptions briefly under Follow-ups / Accepted Risks