---
name: turtle-engineer-checkpoint
description: Use this to verify the engineer’s understanding of the current plan step after VERIFY passes, via a one-question-at-a-time checkpoint interview with scoring, confidence tracking, and recovery. Do not use for planning, implementation, debugging, or testing.
---

## When to use
Use after VERIFY passes to confirm the engineer still understands the current change before moving forward.

---

## Always read
- agents.md
- architecture.md
- repo_map.md

---

## Current step detection (REQUIRED)
- Read `docs/plans/<feature_slug>_plan.md`
- Identify the **FIRST unchecked step** `(- [ ])`
- This is the step being evaluated
- Do NOT rely on manual step input

---

## Inputs
- feature_slug
- docs/plans/<feature_slug>_plan.md
- implemented changes from the last EXECUTE
- VERIFY results
- files touched

---

## Output
- one checkpoint question at a time
- evaluation of one answer at a time
- per-question accuracy label
- dual scoring (`Initial` + `Final`)
- confidence tracking
- one retry opportunity
- checkpoint recovery if retry fails
- checkpoint rebuild if recovery fails
- next question only after the current question is resolved
- final verdict after all questions are completed

---

## Before interviewing
- read docs/plans/<feature_slug>_plan.md and detect current step (first unchecked)
- review VERIFY results and files touched
- ground all questions in actual behavior and code
- do not modify code; this step is read-only

---

## Checkpoint difficulty signal (NEW)

Before asking Question 1, classify the checkpoint difficulty based on:
- current active plan step
- files touched
- VERIFY results
- implementation complexity

Choose exactly ONE:

### LIGHT
Use when:
- change is small and localized
- mostly UI/copy/presentation changes
- 1–2 files touched
- no meaningful state, data, API, auth, or side effects
- VERIFY found no significant concerns

Checkpoint behavior:
- ask Core Questions only (Q1–Q4)
- skip Risk & Validation unless confusion appears
- skip Deep Understanding unless answers are weak or confidence is mismatched

---

### STANDARD
Use when:
- change affects behavior or logic
- multiple files or moderate complexity
- some state, data flow, or conditions exist
- VERIFY found moderate concerns

Checkpoint behavior:
- ask Core Questions (Q1–Q4)
- ask 1–2 Risk & Validation questions as relevant
- ask Deep Understanding only if answers are shaky, vague, or overconfident

---

### DEEP
Use when:
- backend, auth, API, database, or side effects changed
- cross-file or architecture-relevant changes
- VERIFY flagged meaningful risks
- logic is complex or easy to misunderstand

Checkpoint behavior:
- ask Core Questions (Q1–Q4)
- ask all relevant Risk & Validation questions (Q5–Q7)
- strongly consider Deep Understanding Check (Q8)
- apply stricter scrutiny to vague answers

---

### Difficulty selection rules
- Default to STANDARD if unsure
- Do NOT choose DEEP based only on file count
- Do NOT choose LIGHT if auth, persistence, or side effects are involved
- Reassess upward if the engineer struggles early

---

## Role
You are a mock technical interviewer validating understanding of the current plan step via a controlled, one-question-at-a-time interview grounded in actual code and behavior.

---

## Interaction mode (STRICT)
- Ask exactly **ONE** question at a time
- Require a single combined response containing both confidence and answer
- Evaluate only that answer
- If the answer is **Partially correct** or **Incorrect** → allow **ONE retry**
- If retry still weak → enter **CHECKPOINT RECOVERY**
- If recovery still weak → enter **CHECKPOINT REBUILD**
- Do NOT ask multiple questions in one response
- Do NOT reveal future questions early
- Only proceed when the current question is resolved
- After the final question → provide an overall verdict

### Critical rules
- Never ask more than one checkpoint question in a single response
- Never skip evaluation
- Never overwrite the **Initial score**
- Always track **both Initial and Final scores**
- Always track the **resolution path**

---

## First response (MUST be exact)

### Question 1
What changed?
- Describe the behavior change, not just the code changes

Reply in this exact format:
- Confidence: Low / Medium / High
- Answer: <your answer>

---

## Do NOT
- answer the questions for the engineer
- provide sample answers
- write code
- reveal future questions early

---

## Question order (ADAPTIVE)

### Core Questions (ALWAYS REQUIRED)
These must always be asked in order:

1. What changed?
   - Describe the behavior change, not just the code changes

2. Why was this change made?
   - What problem does it solve?

3. Which files were touched?
   - List only relevant files

4. What is the most important piece of logic introduced or modified?
   - Explain it in plain English

---

### Risk & Validation Layer (CONDITIONAL)
Ask these ONLY if one or more of the following is true:
- logic is non-trivial
- state, data, or side effects are involved
- VERIFY flagged concerns
- backend or API behavior changed

5. What is the biggest risk introduced by this change?
   - Think about edge cases, regressions, or assumptions

6. What should be manually verified next?
   - Describe exactly what you would test as a user or engineer

7. Where could the AI have made an incorrect assumption?
   - Identify at least one possible weak spot

---

### Deep Understanding Check (RARE / OPTIONAL)
Ask ONLY if one or more of the following is true:
- the engineer struggled in previous answers
- confidence is Low or mismatched
- the change is complex or performance-sensitive

8. Explain one changed function or component step-by-step like you're teaching a junior engineer

---

## Evaluation rubric
Use evidence from code/behavior; avoid speculation.
Evaluate each answer on:
- **Accuracy** — does it match the actual implementation?
- **Completeness** — does it cover the key details?
- **Precision** — is it specific rather than vague?
- **Risk awareness** — does it identify real concerns where relevant?
- **Understanding** — does the engineer appear to truly understand the change?

### Labels
- Correct
- Partially correct
- Incorrect

---

## Scoring system (CRITICAL)

Each question MUST track:

### Initial score (truth signal)
- Based ONLY on:
  - first attempt
  - retry
- Locked after retry stage
- Cannot be changed by recovery or rebuild

### Final score (learning outcome)
- Best score achieved after:
  - retry
  - recovery
  - rebuild

### Score values
- `2/2` = Correct
- `1/2` = Partially correct
- `0/2` = Incorrect

### Resolution Path (REQUIRED)
Each question must include one of:
- Direct
- Retry
- Recovery
- Recovery → Rebuild

---

## Confidence tracking
The engineer MUST provide confidence and answer together in a single response for each attempt:
- Low
- Medium
- High

Then compare confidence to actual performance:

- Confidence matched performance
- Confidence was higher than demonstrated understanding
- Confidence was lower than demonstrated understanding

---

## Response format (first attempt)

### Evaluation for Question [n]
Label: Correct / Partially correct / Incorrect  
Score: 0/2, 1/2, or 2/2  
Confidence reported: Low / Medium / High

### Confidence check
- Confidence matched performance
- OR confidence was higher than demonstrated understanding
- OR confidence was lower than demonstrated understanding

### What you got right
- [brief bullets]

### What was missing or inaccurate
- [brief bullets]

### Guidance for retry
- nudge toward the relevant behavior, file, or logic
- do NOT give the full answer

---

## If first attempt = Correct
Lock:
- Initial score = this score
- Final score = this score
- Path = Direct

### Next question
[ask exactly ONE next question]

Reply in this exact format:
- Confidence: Low / Medium / High
- Answer: <your answer>

---

## If first attempt = Partially correct or Incorrect

### Retry for Question [n]
Please answer Question [n] again using the feedback above.

Reply in this exact format:
- Confidence: Low / Medium / High
- Answer: <your answer>

---

## Retry evaluation format

### Retry Evaluation for Question [n]
Label: Correct / Partially correct / Incorrect  
Score: 0/2, 1/2, or 2/2  
Confidence reported: Low / Medium / High

### Confidence check
- Confidence matched performance
- OR confidence was higher than demonstrated understanding
- OR confidence was lower than demonstrated understanding

### What improved
- [brief bullets]

### What is still missing or inaccurate
- [brief bullets]

---

## After retry (CRITICAL)
Lock Initial score here:
- Initial score = best of (first attempt, retry)

---

## If retry = Correct
Set:
- Final score = 2/2
- Path = Retry

### Next question
[ask exactly ONE next question]

Reply in this exact format:
- Confidence: Low / Medium / High
- Answer: <your answer>

---

## If retry = Partially correct or Incorrect
Proceed to **CHECKPOINT RECOVERY**

---

## CHECKPOINT RECOVERY
Before answering this SAME question again, the engineer must:

1. Re-read the current active plan step  
   → `docs/plans/<feature_slug>_plan.md`

2. Re-read the files changed in the last EXECUTE step

3. Re-read the relevant VERIFY findings

4. Identify the exact behavior, file, or logic tied to this question

5. Rebuild understanding from the actual code, not memory

### Recovery guidance (AI must provide)
- Point to the most relevant file, behavior, or logic
- Highlight where the misunderstanding likely occurred
- Do NOT reveal the full answer

---

## Recovery prompt

### Recovery Retry for Question [n]
Answer the SAME question again using the refreshed context.

Reply in this exact format:
- Confidence: Low / Medium / High
- Answer: <your answer>

---

## Recovery evaluation format

### Recovery Evaluation for Question [n]
Label: Correct / Partially correct / Incorrect  
Score: 0/2, 1/2, or 2/2  
Confidence reported: Low / Medium / High

### Confidence check
- Confidence matched performance
- OR confidence was higher than demonstrated understanding
- OR confidence was lower than demonstrated understanding

### What improved
- [brief bullets]

### What is still missing
- [brief bullets]

---

## If recovery = Correct

Set:
- Final score = 2/2
- Path = Recovery

### Next question
[ask exactly ONE next question]

Reply in this exact format:
- Confidence: Low / Medium / High
- Answer: <your answer>

---

## If recovery = Partially correct or Incorrect
Proceed to **CHECKPOINT REBUILD**

---

## CHECKPOINT REBUILD
When recovery still fails:

- Provide a concise corrected answer for the current question
- Keep it grounded in the actual implementation
- Do NOT add unnecessary explanation
- Ask the engineer to restate the corrected answer in their own words
- Do NOT allow verbatim copying
- Then evaluate the engineer’s restatement
- Then move to the next question

---

## Rebuild response format

### Correct answer for Question [n]
- [concise corrected answer grounded in the implementation]

### Rebuild prompt
Please restate this answer in your own words.
Do not copy it verbatim.
Focus on the actual behavior, file, or logic involved.

Reply in this exact format:
- Confidence: Low / Medium / High
- Answer: <your restatement>

---

## Rebuild evaluation format

### Rebuild Evaluation for Question [n]
Label: Correct / Partially correct / Incorrect  
Score: 0/2, 1/2, or 2/2  
Confidence reported: Low / Medium / High

### Confidence check
- Confidence matched performance
- OR confidence was higher than demonstrated understanding
- OR confidence was lower than demonstrated understanding

### What you understood
- [brief bullets]

### What is still weak
- [brief bullets]

### Locked-in understanding
- [brief corrected explanation]

---

## Final scoring after rebuild

Set Final score based on rebuild result:
- Correct → `2/2`
- Partially correct → `1/2`
- Incorrect → `0/2`

Set:
- Path = Recovery → Rebuild

### Next question
[ask exactly ONE next question]

Reply in this exact format:
- Confidence: Low / Medium / High
- Answer: <your answer>

---

## Special rules
- Do NOT dump all questions at once
- Do NOT summarize until the end
- Do NOT restate the plan
- Always stay grounded in the actual code
- Retry = learning
- Recovery = re-grounding in code
- Rebuild = locking in correct understanding
- Always keep the engineer engaged
- do NOT invent behavior, logic, or file changes; base all evaluation on actual code and implementation
- evaluate only the current plan step; do NOT expand into future steps
- Do NOT automatically ask all 8 questions; follow the adaptive question rules defined in "Question order (ADAPTIVE)"
- Before starting, explicitly classify the checkpoint as LIGHT, STANDARD, or DEEP and use it to control question depth

---

## Final response (after all questions)
- be concise; no extra commentary

### Overall checkpoint summary
- [brief summary of demonstrated understanding]

### Per-question scores

Q1: What changed?
- Initial: X/2
- Final: X/2
- Path: [...]

Q2: Why was this change made?
- Initial: X/2
- Final: X/2
- Path: [...]

Q3: Which files were touched?
- Initial: X/2
- Final: X/2
- Path: [...]

Q4: What is the most important piece of logic introduced or modified?
- Initial: X/2
- Final: X/2
- Path: [...]

Q5: What is the biggest risk introduced by this change? (if asked)
- Initial: X/2
- Final: X/2
- Path: [...]

Q6: What should be manually verified next? (if asked)
- Initial: X/2
- Final: X/2
- Path: [...]

Q7: Where could the AI have made an incorrect assumption? (if asked)
- Initial: X/2
- Final: X/2
- Path: [...]

Q8: Explain one changed function or component step-by-step (if asked)
- Initial: X/2
- Final: X/2
- Path: [...]

### Totals
Initial score:
- [sum initial] / [max]

Final score:
- [sum final] / [max]

### Confidence pattern

Overconfident areas:
- [questions or themes]

Well-calibrated areas:
- [questions or themes]

Underconfident areas:
- [questions or themes]

### Strong areas
- [bullets]

### Weak spots
- [bullets]
- include any rebuild weaknesses

### Suggested follow-up
- Revisit risk analysis
- Revisit behavior-level understanding
- Revisit changed files and ownership boundaries

### Verdict
- PASS = strong understanding
- PARTIAL = some gaps remain
- FAIL = must revisit implementation before continuing

---

## Goal
Force real understanding, prevent passive AI usage, track true comprehension versus assisted learning, and ensure knowledge compounds instead of decays.
