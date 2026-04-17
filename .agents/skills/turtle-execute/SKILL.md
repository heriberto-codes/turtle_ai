---
name: turtle-execute
description: Use this to implement exactly one scoped plan step or apply minimal fixes after VERIFY fails, using the Current Step Detector to act only on the first unchecked step. Do not use for planning, testing, reviewing, or multi-step changes.
---

## Always read
- agents.md
- architecture.md
- repo_map.md

## External documentation rule
- If implementation depends on third-party framework, library, or SDK behavior, use Context7 to consult current official documentation before making assumptions.
- Prefer repository code as the source of truth for local behavior and architecture.
- If repo code and external docs conflict, call out the conflict explicitly and favor the repo for existing implementation intent.

Modes:
- A = Normal implementation
- B = Fix after VERIFY fails

--------------------------------------------------

## A. Normal Execute

### When to use
Use when implementing the next unchecked plan step for the first time.

### Inputs
- feature_slug
- docs/plans/<feature_slug>_plan.md
- architecture.md
- repo_map.md
- relevant existing code

### Output
- code changes for one scoped step only OR an explicit already_satisfied determination
- list of modified files
- short implementation summary
- assumptions made
- execution outcome: implemented | already_satisfied
- confirmation that only the current plan step was evaluated or implemented

EXECUTE

You are a Staff Software Engineer implementing a scoped task.

Task
Implement ONLY the next unchecked step from:
docs/plans/<feature_slug>_plan.md

Current step detection (REQUIRED):
- Read docs/plans/<feature_slug>_plan.md
- Identify the FIRST unchecked step (- [ ])
- This is the ONLY step allowed for implementation
- Do NOT ask the user for step input

## Before coding
- read docs/plans/<feature_slug>_plan.md and identify the FIRST unchecked step
- read architecture.md and repo_map.md
- inspect only the relevant existing code for this step
- do not modify plan state; this step remains unchecked

## Step reconciliation rule (CRITICAL)
- If the FIRST unchecked step already appears fully satisfied in the current working tree, do NOT force unnecessary code edits.
- In that case, return the explicit outcome: `already_satisfied`
- Treat this as state reconciliation, not completion.
- Do NOT mark the step complete.
- Do NOT skip VERIFY, TEST, or PLAN STEP UPDATE.
- VERIFY must review the existing repo state for that step before progression continues.

Fail-safe (CRITICAL):
- Before proceeding, scan docs/plans/<feature_slug>_plan.md for unchecked steps (- [ ])
- If NO unchecked steps exist:
  - STOP immediately
  - Output exactly:
    "No remaining unchecked steps. Execution is complete. Proceed to FINALIZATION."
- Do NOT implement anything if all steps are already marked [x]

Execution Rules (STRICT):
- Implement ONE task only
- If the task feels larger than a single step, STOP and say so
- Touch ONLY the files required for this task
- Do NOT refactor unrelated code
- Do NOT introduce new patterns or frameworks
- Do NOT solve future steps
- Follow existing repo conventions exactly
- Keep changes minimal and reversible
- The current step MUST remain unchecked (- [ ])
- Do NOT modify plan state (no [ ] → [x])
- Plan state changes are handled ONLY by PLAN STEP UPDATE
- do NOT invent files, modules, or behaviors; base changes on existing code and patterns
- stay within the current step scope; flag if broader changes are required
- prefer existing utilities and helpers over creating new ones
- if no code change is required because the current unchecked step is already implemented, report `already_satisfied` explicitly instead of returning an ambiguous no-op

## Output Format
1. Execution outcome
- `implemented` or `already_satisfied`

2. Files changed

3. Code changes / existing evidence

4. Short explanation of what was implemented or why the step already appears satisfied

5. Assumptions made

## Stop Conditions
- Task is complete
- OR task is too large for a single step → clearly say so
- OR you are blocked → clearly state blocker
- OR required changes exceed current step scope → STOP and state scope violation

Do NOT:
- Add tests (that is a separate step)
- Perform a review (that is VERIFY)
- Continue beyond this task

Goal:
Make a small, correct, isolated change that is easy to understand and verify.

When the step is implemented:
- summarize the changes made
- list modified files
- confirm that only the current plan step was implemented

When the step is already satisfied:
- state that no new code changes were required
- identify the files that already satisfy the step
- confirm that plan state remains unchanged
- explicitly instruct VERIFY to review the current repo state for this step

--------------------------------------------------

## B. Execute — Verify Fix Mode

### When to use
Use only when VERIFY returns FAIL and the current plan step needs follow-up fixes.

### Inputs
- feature_slug
- docs/plans/<feature_slug>_plan.md
- agents.md
- architecture.md
- repo_map.md
- latest VERIFY findings
- files changed in last EXECUTE

### Output
- scoped code fixes for the current step only
- summary of fixes
- modified files
- validation notes
- any minimal plan update needed
- confirmation that no future steps were implemented

EXECUTE (VERIFY FIX MODE)

Use only when VERIFY returns FAIL.

You are a Staff Software Engineer implementing follow-up fixes after a VERIFY review.

Task
Address all issues identified in the latest VERIFY step for the current feature and current plan step only.

Fail-safe (CRITICAL):
- Before proceeding, scan docs/plans/<feature_slug>_plan.md for unchecked steps (- [ ])
- If NO unchecked steps exist:
  - STOP immediately
  - Output exactly:
    "No remaining unchecked steps. Execution is complete. Proceed to FINALIZATION."
- Do NOT implement anything if all steps are already marked [x]

Context
- Feature: <feature_slug>
- Plan file: docs/plans/<feature_slug>_plan.md

Current step detection (REQUIRED):
- Read docs/plans/<feature_slug>_plan.md
- Identify the FIRST unchecked step (- [ ])
- This is the step being fixed
- Do NOT rely on manual step input

## Before fixing
- read docs/plans/<feature_slug>_plan.md and detect current step (first unchecked)
- read agents.md, architecture.md, and repo_map.md
- review latest VERIFY findings and prior changes
- do not modify plan state; this step remains unchecked

Responsibilities
- Fix all implementation issues identified in VERIFY
- Ensure alignment with the current plan step and repo conventions
- Preserve intended behavior unless VERIFY says behavior is incorrect
- Resolve plan deviations explicitly

Constraints
- Stay strictly within the current plan step
- Do NOT introduce new features
- Do NOT expand scope
- Do NOT add tests yet
- Do NOT refactor unrelated code
- Follow agents.md rules and protected patterns
- Prefer minimal, safe, reversible changes
- Maintain existing architecture and repo patterns
- do NOT invent fixes; base changes on VERIFY findings and code evidence
- prefer existing utilities and helpers over creating new ones

## Decision Rules
- If implementation is incorrect → fix the code
- If implementation is correct but VERIFY is incorrect → note the discrepancy and proceed cautiously
- If the current plan step is incorrect → propose a minimal plan update for the current step only
- If VERIFY findings conflict with the plan → choose the safest repo-consistent approach and document it
- If the required fix exceeds current step scope → STOP and report scope violation

## Stop Conditions
- all VERIFY issues for this step are fixed
- OR the required fix exceeds current step scope → STOP and report scope violation
- OR VERIFY findings are unclear or conflicting → STOP and state the issue clearly

## Output Format
1. Summary of fixes
- what issues were addressed

2. Files changed
- full paths of modified files

3. Code changes
- what changed and why

4. Proposed plan updates (if any)
- exact proposed updates to docs/plans/<feature_slug>_plan.md
- current-step changes only; do NOT modify plan state

5. Validation notes
- why the fixes now align with VERIFY expectations

6. Remaining risks / follow-ups
- what should be validated in TEST or later steps

Completion confirmation:
- confirm only the current plan step was fixed
- confirm no future steps were implemented
- confirm no plan state changes were made
- confirm any plan updates were proposed only, not applied

Do NOT:
- Write tests
- Skip any VERIFY issue
- Proceed beyond this step
