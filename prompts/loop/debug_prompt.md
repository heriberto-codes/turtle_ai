/debug_prompt

Modes:
- A = Diagnose the failure and identify the minimal safe fix
- B = Apply the fix identified in DEBUG mode A

--------------------------------------------------

## A. Debug / Diagnose

### When to use
Use when VERIFY, TEST, ENGINEER CHECKPOINT, or runtime/manual validation reveals a problem and the root cause is not yet clear.

### Trigger conditions
- VERIFY returns FAIL
- TEST reveals failing or missing coverage
- ENGINEER CHECKPOINT reveals lack of understanding or inconsistency
- Runtime or manual validation reveals incorrect behavior

---

## 🔀 DEBUG ROUTING RULE (CRITICAL)

After diagnosing the issue, you MUST explicitly determine where the fix belongs.

DEBUG does NOT automatically mean "fix the code".

### Required decision

You MUST choose ONE of the following routing paths:

#### 1. Implementation Issue
- Root cause: incorrect or incomplete code
- Action:
  → route to EXECUTE (DEBUG FIX)

#### 2. Plan Mismatch Issue
- Root cause: implementation does not match plan intent
- Action:
  → route to EXECUTE (VERIFY FIX MODE)

#### 3. Test Issue
- Root cause: incorrect, outdated, or missing tests
- Action:
  → route to TEST

#### 4. Unclear or Mixed Issue
- Root cause: cannot isolate cleanly
- Action:
  → default to EXECUTE (DEBUG FIX)
  → clearly state assumptions

### Output requirements

You MUST include in your DEBUG output:

- Selected routing path
- Short justification
- Why other paths were NOT chosen (if applicable)

### Core principle

Always fix the correct layer:

- Code → EXECUTE
- Plan alignment → EXECUTE (VERIFY FIX MODE)
- Tests → TEST

Never mix layers.

### Failure modes prevented

- fixing tests when code is wrong
- fixing code when tests are wrong
- drifting from plan intent
- infinite debug loops

---

### Inputs required
- feature slug
- docs/plans/<feature_slug>_plan.md
- full error output
- logs
- failing behavior
- relevant code
- related tests

### Output expected
- observed issue
- root cause
- failure category
- files involved
- minimal safe fix recommendation
- risks
- validation steps
- routing decision

DEBUG

You are a Senior Debugging Engineer.

Task
Diagnose the failing behavior, identify the true root cause, and recommend the smallest safe fix.

Context
- Feature: <feature_slug>

Current step detection (REQUIRED):
- Read docs/plans/<feature_slug>_plan.md
- Identify:
  - FIRST unchecked step (- [ ]) → active step
  - LAST checked step (- [x]) → last completed step
- Use whichever is relevant to the failure context
- Do NOT rely on manual step input

Error output
<paste FULL output here>

Process
1. State observed failure
2. Identify root cause
3. Identify non-blocking noise
4. Identify failure category:
   - app / test / config / env / mocks
5. Identify files involved
6. Validate scope of the fix
   - confirm the fix applies ONLY to the current plan step
   - if the fix requires changes beyond this step, flag it as a scope violation
7. Propose minimal safe fix
8. Identify risks
9. Suggest validation steps
10. Confirm reproducibility
   - can the issue be reliably reproduced?
   - if not, state uncertainty and assumptions
11. Decide routing (MANDATORY):
   - DEBUG FIX mode
   - VERIFY FIX MODE
   - OR TEST

Output

Observed issue
- [concise]

Root cause
- [concise]

Non-blocking noise
- [concise]

Failure category
- [app / test / config / env / mocks]

Files involved
- [paths]

Scope check
- [within current step / scope violation]

Fix
- [minimal change]

Why this fix
- [short reasoning]

Risks
- [bullets]

Validation
- [commands or tests to rerun]

Reproducibility
- [reproducible / partially reproducible / uncertain]
- [assumptions if needed]

Routing decision
- Selected path: [DEBUG FIX / VERIFY FIX / TEST]
- Why this path:
  - [short justification]
- Why not others:
  - [brief explanation if ambiguous]

Rules
- no large rewrites
- prefer smallest fix
- stay within step scope
- state assumptions clearly
- do NOT modify plan state (no [ ] → [x])
- plan updates are handled ONLY by PLAN STEP UPDATE

Be concise.

--------------------------------------------------

## B. Debug Fix / Apply Minimal Fix

### When to use
Use only after DEBUG mode A identifies the fix.

### Inputs required
- feature slug
- docs/plans/<feature_slug>_plan.md
- agents.md
- architecture.md
- repo_map.md
- DEBUG findings

### Output expected
- minimal safe fix
- files changed
- reasoning
- validation notes

EXECUTE (DEBUG FIX)

You are a Staff Software Engineer applying the fix identified in DEBUG.

Task
Implement the minimal safe fix.

Current step detection (REQUIRED):
- Read docs/plans/<feature_slug>_plan.md
- Identify:
  - FIRST unchecked step (- [ ]) → active step
  - LAST checked step (- [x]) → last completed step
- Use whichever is relevant to the failure context
- Do NOT rely on manual step input

Rules
- stay within current step
- no scope expansion
- no unrelated changes
- follow repo patterns
- minimal, reversible changes
- do NOT modify plan state (no [ ] → [x])
- plan updates are handled ONLY by PLAN STEP UPDATE

Output
1. Files changed
2. Fix applied
3. Why it works
4. Risks
5. Validation steps

Completion confirmation
- stayed within scope
- no unrelated changes
- no plan state was modified