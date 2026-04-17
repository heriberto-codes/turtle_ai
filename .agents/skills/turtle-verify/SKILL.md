---
name: turtle-verify
description: Use this to review the latest implementation for the current plan step and determine PASS or FAIL based on correctness, scope, risks, and alignment with the plan. Do not implement fixes or expand scope.
---

## Always read
- agents.md
- architecture.md
- repo_map.md

## External documentation rule
- If correctness review depends on third-party framework, library, or SDK behavior, use Context7 to consult current official documentation before making assumptions.
- Prefer repository code as the source of truth for local behavior and architecture.
- If repo code and external docs conflict, call out the conflict explicitly and favor the repo for existing implementation intent.

## When to use
Use immediately after EXECUTE to review the latest implementation for the current plan step.

## Current step detection (REQUIRED)

- Read docs/plans/<feature_slug>_plan.md
- Identify the FIRST unchecked step (- [ ])
- This is the current step under review
- Do NOT rely on manual input

## Inputs
- feature_slug
- docs/plans/<feature_slug>_plan.md
- EXECUTE outcome: implemented | already_satisfied
- files changed in last EXECUTE (if any)
- files or code evidence identified by EXECUTE when outcome = already_satisfied
- implemented code for the current step

## Output
- review summary
- what is correct
- issues found
- risk summary
- required fixes
- clear verdict: PASS or FAIL

VERIFY

You are a Senior Reviewer performing a code review.

## Task

Review the current plan step using the correct evidence source:
- if EXECUTE outcome = `implemented`, review the changes from the last EXECUTE step
- if EXECUTE outcome = `already_satisfied`, review the files and repo state that already satisfy the current unchecked step

## Before reviewing
- read docs/plans/<feature_slug>_plan.md
- identify the FIRST unchecked step
- understand intended behavior of that step
- determine evidence source from EXECUTE outcome
- if outcome = `implemented`, review ONLY files changed in last EXECUTE
- if outcome = `already_satisfied`, review ONLY the files identified as existing evidence for the current unchecked step
- compare expected vs actual implementation
- do not modify plan state

## State reconciliation rule (CRITICAL)
- `already_satisfied` is a valid EXECUTE outcome when the first unchecked step is already present in the working tree.
- This is NOT an automatic PASS.
- VERIFY must confirm that the current unchecked step is actually satisfied by the existing repo state.
- If the evidence is incomplete, ambiguous, or outside current-step scope, return FAIL.
- If the evidence fully satisfies the current unchecked step, PASS is allowed even when the latest EXECUTE made no new file changes.

## Review for

### 1. Correctness
- implements intended behavior
- logical bugs or inconsistencies

### 2. Scope Control
- changes limited to current step
- no unnecessary edits or drift

### 3. Edge Cases
- validation present
- null/undefined handling
- boundary conditions

### 4. Regression Risk
- impact on existing behavior
- shared logic affected

### 5. Code Quality
- aligns with repo patterns
- readable and maintainable
- avoids duplication/over-complexity

### 6. Assumptions
- implicit assumptions identified and validated

## Rules
- stay strictly within current step
- do NOT invent behavior; base review on actual code
- do NOT implement fixes
- do NOT expand scope
- do NOT fail solely because the latest EXECUTE made no file changes when EXECUTE explicitly returned `already_satisfied`

## Output Format

✅ What is correct
- [bullets]

⚠️ Issues found
- [specific + actionable]

🧠 Risk summary
- [biggest risk]

📌 Required fixes
- [clear next actions]

## Verdict

- PASS → safe to proceed
- FAIL → must fix before continuing

## Stop Conditions
- PASS → safe to proceed
- FAIL → must fix before continuing

## Constraints

- Do NOT rewrite full code
- Do NOT implement fixes (handled in EXECUTE B)
- Do NOT expand scope
- Stay strictly within THIS step
- do NOT add tests

## Goal

Ensure the change is correct, minimal, and safe before moving forward.
