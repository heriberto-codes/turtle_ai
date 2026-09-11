---
name: turtle-engineer-checkpoint
description: Use this after VERIFY passes to confirm the engineer understands the current plan step through a bounded, one-question-at-a-time teach-back checkpoint. Do not use for planning, implementation, debugging, or testing.
---

# Engineer Checkpoint

Confirm that the engineer understands the current change without turning the checkpoint into an open-ended interview. Evaluate material understanding, not memorization or exhaustive recall.

## Always read

- `agents.md`
- `architecture.md`
- `repo_map.md`

## Current step detection

- Read `docs/plans/<feature_slug>_plan.md`.
- Identify the first unchecked step (`- [ ]`). This is the only step being evaluated.
- Do not rely on manually supplied step numbers.

## Inputs

- `feature_slug`
- the active plan step
- implementation changes from the latest EXECUTE
- VERIFY results
- files touched

## Before interviewing

- Review the active plan step, VERIFY results, and relevant changed code.
- Establish the correct behavior, purpose, important logic, implementation location, and meaningful risks.
- Ground every evaluation in the implementation; do not speculate or invent behavior.
- Do not modify code or plan state. This skill is read-only.

## Difficulty and question limit

Choose one difficulty before Question 1 and do not increase it during the checkpoint. Difficulty controls question depth, not question count. Every checkpoint asks a maximum of three questions.

### LIGHT

Use when the change is small and localized, usually involving one or two files, with no meaningful state, persistence, API, auth, or side effects and no significant VERIFY concerns. Keep the selected questions focused on observable behavior, primary logic, and basic validation.

### STANDARD

Use when behavior or moderate logic changed, multiple files participate, state or conditions are involved, or VERIFY identified a moderate concern. Include relevant conditions, ownership, and regression risk in the selected questions.

### DEEP

Use when backend behavior, auth, APIs, databases, persistence, side effects, architecture boundaries, or complex logic changed, or VERIFY identified a meaningful risk. Make the selected questions probe the most important data flow, boundary, side effect, or failure mode.

### Selection rules

- Default to STANDARD when the evidence does not clearly support LIGHT or DEEP.
- Do not choose DEEP from file count alone.
- Do not choose LIGHT when auth, persistence, or side effects are involved.
- Before Question 1, randomly select one grounded question from each category in the question pool, then randomize their order.
- Adapt the chosen prompts to the active step and actual code instead of repeating the example wording mechanically.
- When recent checkpoint history is available, avoid repeating the immediately previous prompt wording and order.
- Select all three questions before starting, but reveal only the current question.
- If an earlier answer fully demonstrates the understanding sought by a later selected question, remove that later question, note the coverage under the earlier question, and do not replace it.
- Never ask more than three questions in one checkpoint.

## Randomized question pool

Ask exactly one question at a time. Randomly choose one question from each category and shuffle the three-question order.

### Behavior and intent

- Describe the observable behavior change and the problem it solves.
- Compare the behavior before and after this step. Why was the difference needed?
- From the user or system perspective, what outcome changed and why?

### Implementation and logic

- Explain the most important changed logic and identify where it is implemented.
- Trace the main control or data path introduced by this step in plain English.
- Which function or component owns the changed behavior, and how does it produce the result?

### Risk and validation

- What is the biggest material regression risk, and how would you verify it?
- Which implementation assumption is most likely to be wrong, and what evidence would confirm it?
- What edge case or failure mode matters most, and how should it be tested manually?

Do not use a standalone file-recall question. Secondary file omissions are not evidence of misunderstanding when the engineer can locate and explain the primary implementation.

## Material-understanding standard

Evaluate answers using actual code and behavior.

A **material gap** is a factual error or missing concept that changes the engineer's mental model of:

- the implemented behavior
- why the change exists
- the key logic or its primary ownership
- state, data flow, persistence, API, auth, or side effects when relevant
- a significant risk or validation requirement when that topic is asked

The following are **non-material omissions** and must not block progress by themselves:

- different wording that preserves the correct meaning
- missing secondary details or secondary file names
- an answer that is concise but demonstrates the correct mental model
- minor imprecision that would not change implementation, debugging, or validation decisions

## Labels, resolution, and confidence

Label every answer:

- `Correct` — materially accurate
- `Partially correct` — some correct understanding, with either material or non-material omissions
- `Incorrect` — the central mental model is wrong or absent

Track each asked question with:

- `Resolution: Resolved | Unresolved`
- `Path: Direct | Retry | Teach-back`

Record confidence only for the initial answer:

- `Low`
- `Medium`
- `High`

Compare the initial confidence with demonstrated understanding as `Matched`, `Overconfident`, or `Underconfident`. Confidence is a reflection signal, not a pass requirement; an accurate low-confidence answer can pass.

## Bounded interaction contract

Each question has at most three engineer answers:

1. initial answer
2. one guided retry, only when a material gap remains
3. one teach-back, only when the retry still has a material gap

Never add another recovery stage, another restatement, or a same-session checkpoint restart.

### Initial answer

Require:

```text
- Confidence: Low / Medium / High
- Answer: <your answer>
```

Evaluate the answer concisely:

```markdown
### Evaluation for Question [n]
Label: Correct / Partially correct / Incorrect
Confidence: Low / Medium / High — Matched / Overconfident / Underconfident

What you understood:
- [brief evidence]

Material gap:
- [brief gap, or "None"]
```

- If there is no material gap, set `Resolution: Resolved` and `Path: Direct`, then ask the next selected question.
- A `Partially correct` answer with only non-material omissions resolves directly.
- If a material gap remains, give a focused hint pointing to the relevant behavior, plan statement, VERIFY finding, or primary code location without supplying the complete answer. Then request one retry.

### Guided retry

Ask:

```markdown
### Retry for Question [n]
[focused guidance]

Please answer Question [n] once more. Confidence is not required again.

- Answer: <your answer>
```

Evaluate only what changed:

```markdown
### Retry evaluation for Question [n]
Label: Correct / Partially correct / Incorrect

What improved:
- [brief evidence]

Material gap:
- [brief gap, or "None"]
```

- If no material gap remains, set `Resolution: Resolved` and `Path: Retry`, then continue.
- If a material gap remains, proceed immediately to teach-back. Do not insert a separate recovery reread cycle.

### Teach-back

Provide the concise corrected explanation grounded in the implementation, then ask for one restatement:

```markdown
### Correct explanation for Question [n]
- [concise corrected explanation]

### Teach-back
Restate this in your own words without copying it verbatim. Focus on the actual behavior, purpose, logic, or risk being tested.

- Answer: <your restatement>
```

Evaluate whether the restatement now demonstrates the correct material mental model.

- If it does, set `Resolution: Resolved` and `Path: Teach-back`, then continue.
- If it does not, set `Resolution: Unresolved` and `Path: Teach-back`, stop the checkpoint immediately, and return `FAIL` with the exact files or findings to review.
- Do not continue to later questions after an unresolved teach-back.

## First response

The first response must contain the fixed difficulty, three-question limit, brief evidence for the classification, and the first randomly selected question:

```markdown
### Checkpoint setup
Difficulty: LIGHT / STANDARD / DEEP
Question limit: 3
Selection: Randomized
Reason: [one concise evidence-based sentence]

### Question 1
[first randomly selected and implementation-grounded question]

Reply in this exact format:
- Confidence: Low / Medium / High
- Answer: <your answer>
```

## Moving between questions

After resolving a question, report its resolution and ask exactly one next question:

```markdown
Resolution: Resolved
Path: Direct / Retry / Teach-back

### Question [n]
[selected question]

Reply in this exact format:
- Confidence: Low / Medium / High
- Answer: <your answer>
```

After the final selected question resolves, return the PASS summary instead of another question.

## Final outcomes

Use binary verdicts only. There is no checkpoint-level `PARTIAL` verdict.

### PASS

Return PASS only when every asked question is resolved. Initial errors remain visible as learning history but do not override successfully demonstrated understanding.

```markdown
### Overall checkpoint summary
- [brief summary of demonstrated understanding]

### Question outcomes
Q1: [question]
- Attempts: Initial [label] / Retry [label, if used] / Teach-back [label, if used]
- Resolution: Resolved
- Path: Direct / Retry / Teach-back
- Initial confidence: Low / Medium / High — Matched / Overconfident / Underconfident

[repeat only for questions asked]

### Strong areas
- [brief bullets]

### Follow-up learning
- [non-blocking details worth revisiting, or "None"]

### Verdict
PASS — every material understanding requirement was resolved.
```

### FAIL

Return FAIL immediately when a teach-back remains materially incorrect or incomplete.

```markdown
### Checkpoint stopped
Question: [n and text]
Resolution: Unresolved
Path: Teach-back

### Material misunderstanding
- [precise description]

### Review target
- [specific plan section, VERIFY finding, and/or primary file and logic]

### Verdict
FAIL — revisit the implementation before starting a fresh checkpoint session.
```

## Constraints

- Do not answer for the engineer before the teach-back stage.
- Do not provide sample answers during the initial or guided-retry stages.
- Do not ask more than one question in a response.
- Do not ask more than three questions in the entire checkpoint.
- Do not reuse a fixed question order; randomize the prompt selection and order before every checkpoint.
- Do not evaluate future plan steps.
- Do not convert non-material omissions into blocking failures.
- Do not proceed to TEST unless the final verdict is PASS.

## Goal

Require real, implementation-grounded understanding while giving every checkpoint a predictable maximum length and a clear advancement decision.
