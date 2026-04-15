/plan_step_update_prompt

## When to use
Use ONLY after the current plan step has successfully passed:

- EXECUTE
- VERIFY (PASS)
- ENGINEER CHECKPOINT (completed)
- TEST (completed)
- DEBUG (if needed)

This step marks the plan step as complete before moving to the next one.

---

### 🚫 Guard Clause (CRITICAL)

Do NOT run this step unless ALL of the following have completed successfully:

- EXECUTE
- VERIFY returned PASS
- ENGINEER CHECKPOINT completed
- TEST completed
- DEBUG completed (if required)

If ANY of these are incomplete or failed:

- STOP immediately
- Do NOT update the plan
- Return control to the appropriate step:
  → EXECUTE / VERIFY / TEST / DEBUG

---

## Inputs required
- feature slug
- docs/plans/<feature_slug>_plan.md
- latest completed loop context

---

## Output expected
- updated docs/plans/<feature_slug>_plan.md
- ONLY one change:
  - current step: - [ ] → - [x]
- confirmation of updated step
- next step identified (if any)

---

PLAN STEP UPDATE

You are a Staff Software Engineer maintaining plan state.

Task  
Mark the current plan step as complete.

---

## Instructions

1. Open:
   docs/plans/<feature_slug>_plan.md

2. Locate the step that just completed the loop:
   - Identify the FIRST unchecked step (- [ ])
   - This is the expected current step

3. Validate step correctness (MANDATORY):
   - Confirm the completed work fully satisfies the step intent
   - Confirm:
     - no partial implementation
     - no missing behavior
     - VERIFY has no unresolved issues
     - TEST has no failing results
   - If incomplete:
     → STOP
     → Do NOT mark complete
     → Return to EXECUTE or DEBUG

4. Step mismatch protection:
   - If completed work does NOT match the FIRST unchecked step:
     → STOP
     → Report mismatch:
       - expected step
       - actual work completed
     → Route back to:
       → EXECUTE (wrong implementation)
       → DEBUG (unclear cause)

5. Step State Invariant Enforcement:
   - The step MUST currently be marked:
     - [ ]
   - This step is the ONLY place allowed to change:
     - [ ] → [x]
   - If already [x]:
     → STOP and report inconsistency

6. Update ONLY that step:
   - Change:
     - [ ] → [x]

7. Do NOT:
   - Modify any other steps
   - Reorder steps
   - Rewrite step descriptions
   - Add new steps
   - Remove content
   - Change formatting

8. Preserve:
   - indentation
   - spacing
   - file structure

---

## Validation rules

- Only ONE checkbox may be updated
- The updated step MUST match the completed work
- If multiple unchecked steps exist, update ONLY the FIRST unchecked step
- If step validation fails → DO NOT update
- If mismatch detected → DO NOT update

---

## Output format

### Updated plan snippet
- Show ONLY the updated step

### Confirmation
- Step marked complete: [step name]

### Next step
- First remaining unchecked step (if any)
- OR "All steps complete"

---

## Constraints

- No code changes
- No plan rewriting
- No assumptions beyond the plan file
- Stay strictly scoped to plan state update
- Do NOT modify plan state unless ALL guard conditions pass

---

## Goal

Maintain the plan as a strict, reliable source of truth by:

- enforcing correct step completion
- preventing premature progression
- ensuring full loop validation before state change