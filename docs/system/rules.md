# Turtle AI Global Rules

These rules protect the workflow from partial execution, unclear ownership, and invalid state transitions.

## Step State Invariant

| Constraint | Description |
| --- | --- |
| Active step remains unchecked | The current step stays `[ ]` during the loop. |
| Completion gating | A step is marked `[x]` only after the full loop passes. |
| Loop order enforcement | `EXECUTE -> VERIFY -> ENGINEER CHECKPOINT -> TEST -> PLAN STEP UPDATE`; use DEBUG for recovery when needed. |
| Single write authority | Only `turtle-plan-step-update` can change `[ ]` to `[x]`. |
| Scope of work | `EXECUTE`, `VERIFY`, `ENGINEER CHECKPOINT`, and `TEST` operate on the first unchecked step. |

Rules:

- Never mark a step complete during `EXECUTE`, `VERIFY`, `ENGINEER CHECKPOINT`, `TEST`, or `DEBUG`.
- Always complete the full loop before updating plan state.
- Do not skip any required stage in the loop; DEBUG is conditional recovery.

## Debug Routing

When something fails, identify what is actually wrong before changing anything.

| Problem | Fix |
| --- | --- |
| The code is incorrect | Fix the code through the implementation path. |
| The implementation does not match the plan | Adjust the code to match the plan. |
| The test is incorrect | Fix the test through the test path. |

Rules:

- Do not guess. Identify the root cause first.
- Do not change multiple layers at once.
- Fix only what is actually broken.
- Code or plan-alignment fixes return through EXECUTE, VERIFY, ENGINEER CHECKPOINT, and TEST. Test fixes return to TEST and require fresh applicable validation results.
- Do not proceed from diagnosis or a fix directly to PLAN STEP UPDATE. Resolve failed gates first and retain explicit checkpoint PASS evidence.

## Engineer Checkpoint

`ENGINEER CHECKPOINT` is a required comprehension gate before moving forward.

The engineer must be able to explain:

- what was implemented
- why it works
- how it fits into the system

If the engineer cannot demonstrate materially correct understanding after the bounded retry and multiple-choice recovery flow:

- stop the workflow
- do not proceed to `TEST` or `PLAN STEP UPDATE`
- revisit the implementation

Rules:

- This step enforces understanding, not correctness.
- Passing `VERIFY` does not guarantee passing `ENGINEER CHECKPOINT`.
- Only an explicit `PASS` permits the workflow to proceed to `TEST`; there is no checkpoint-level `PARTIAL` outcome.
- Initial mistakes do not block progress when the engineer resolves the material misunderstanding during the bounded checkpoint.
- Confidence is tracked for reflection but does not determine the verdict.
- The engineer is the source of truth, not the AI.

## Required Loop

Always follow this sequence:

```text
EXECUTE -> VERIFY -> ENGINEER CHECKPOINT -> TEST -> PLAN STEP UPDATE
```

`DEBUG` branches from failures when diagnosis is needed and routes back to the appropriate repair phase. It is not required on a successful pass through the loop.

`PLAN STEP UPDATE` is required. A step is not complete until it runs.
