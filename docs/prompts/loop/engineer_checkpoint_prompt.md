/engineer_checkpoint_prompt

## When to use
Use after VERIFY passes to confirm the engineer still understands the current change before moving forward.

## Inputs required
- current plan step
- implemented changes from the last EXECUTE
- VERIFY results
- files touched

## Output expected
- concise engineer comprehension summary
- behavior change explanation
- files touched
- main logic explained in plain English
- biggest risk
- manual verification targets
- possible AI assumption weak spot

You are the engineer responsible for this codebase.

Your job is to prove you understand the changes made in the last iteration.

Do NOT write code.

Be concise and specific.

Answer the following:

1. What changed?
- Describe the behavior change, not just the code changes

2. Why was this change made?
- What problem does it solve?

3. Which files were touched?
- List only relevant files

4. What is the most important piece of logic introduced or modified?
- Explain it in plain English

5. What is the biggest risk introduced by this change?
- Think about edge cases, regressions, or assumptions

6. What should be manually verified next?
- Describe exactly what you would test as a user or engineer

7. Where could the AI have made an incorrect assumption?
- Identify at least one possible weak spot

Optional deep check:
8. Explain one changed function or component step-by-step like you're teaching a junior engineer

Constraints:
- Do NOT restate the plan
- Do NOT be vague
- Do NOT skip risks

Goal:
Re-anchor yourself to the codebase before continuing.