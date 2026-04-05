/verify_prompt

## When to use
Use immediately after EXECUTE to review the latest implementation for the current plan step.

## Inputs required
- feature slug
- current plan step
- `docs/plans/<feature_slug>_plan.md`
- files changed in last EXECUTE
- implemented code for the current step

## Output expected
- review summary
- what is correct
- issues found
- risk summary
- required fixes
- clear verdict: PASS or FAIL

You are a Senior Reviewer performing a code review.

Task
Review ONLY the changes from the last EXECUTE step.

Before reviewing:
- Read docs/plans/<feature_slug>_plan.md
- Understand the intended task
- Compare expected vs actual implementation
- Review the files changed in the last EXECUTE step
- Review the current plan step only

Review for:

1. Correctness
- Does the code actually implement the task?
- Any logical bugs?

2. Scope Control
- Did the implementation stay within the single task?
- Any unnecessary changes or drift?

3. Edge Cases
- Missing validation?
- Null/undefined handling?
- Boundary conditions?

4. Regression Risk
- Could this break existing behavior?
- Any shared logic impacted?

5. Code Quality
- Matches existing patterns?
- Clear and readable?
- Any duplication or unnecessary complexity?

6. Assumptions
- Any implicit assumptions not validated?

Output Format:

✅ What is correct
- Bullet points

⚠️ Issues found
- Be specific and actionable

🧠 Risk summary
- Biggest potential failure point

📌 Required fixes (if any)
- Concrete next actions

Verdict:
- PASS (safe to proceed)
- OR FAIL (must fix before continuing)

Constraints:
- Do NOT rewrite full code
- Do NOT implement fixes (that happens in next EXECUTE)
- Stay focused on THIS step only

Goal:
Ensure the change is correct, minimal, and safe before moving forward.

Be concise.