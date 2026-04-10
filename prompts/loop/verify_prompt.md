/verify_prompt

## When to use
Use immediately after EXECUTE to review the latest implementation for the current plan step.

---

## Current step detection (REQUIRED)

- Read docs/plans/<feature_slug>_plan.md
- Identify the FIRST unchecked step (- [ ])
- This is the current step under review
- Do NOT rely on manual input

---

## Inputs required
- feature slug
- docs/plans/<feature_slug>_plan.md
- files changed in last EXECUTE
- implemented code for the current step

---

## Output expected
- review summary
- what is correct
- issues found
- risk summary
- required fixes
- clear verdict: PASS or FAIL

---

VERIFY

You are a Senior Reviewer performing a code review.

---

## Task

Review ONLY the changes from the last EXECUTE step for the current plan step.

---

## Before reviewing

1. Read docs/plans/<feature_slug>_plan.md
2. Identify the FIRST unchecked step (- [ ])
3. Understand the intended behavior of that step
4. Review ONLY the files changed in the last EXECUTE step
5. Compare expected vs actual implementation

---

## Review for

### 1. Correctness
- Does the code actually implement the intended behavior?
- Any logical bugs?

### 2. Scope Control
- Did the implementation stay within the single step?
- Any unnecessary changes or drift?

### 3. Edge Cases
- Missing validation?
- Null/undefined handling?
- Boundary conditions?

### 4. Regression Risk
- Could this break existing behavior?
- Any shared logic impacted?

### 5. Code Quality
- Matches existing patterns?
- Clear and readable?
- Any duplication or unnecessary complexity?

### 6. Assumptions
- Any implicit assumptions not validated?

---

## Output Format

✅ What is correct
- [bullets]

⚠️ Issues found
- [specific + actionable]

🧠 Risk summary
- [biggest risk]

📌 Required fixes
- [clear next actions]

---

## Verdict

- PASS → safe to proceed
- FAIL → must fix before continuing

---

## Constraints

- Do NOT rewrite full code
- Do NOT implement fixes (handled in EXECUTE B)
- Do NOT expand scope
- Stay strictly within THIS step

---

## Goal

Ensure the change is correct, minimal, and safe before moving forward.