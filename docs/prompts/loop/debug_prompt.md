/debug_prompt

Modes:
- A = Diagnose the failure and identify the minimal safe fix
- B = Apply the fix identified in DEBUG mode A

--------------------------------------------------

A. Debug / Diagnose

## When to use
Use when EXECUTE, VERIFY, TEST, or manual validation reveals a problem and the root cause is not yet clear.

## Inputs required
- feature slug
- current plan step
- docs/plans/<feature_slug>_plan.md (if relevant)
- full error output
- logs
- failing behavior
- relevant code
- related tests

## Output expected
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
- Current step: <step_number_or_name>

Error output
<paste FULL output here>

Process
1. State observed failure
2. Identify root cause
3. Identify non-blocking noise
4. Identify failure category:
   - app / test / config / env / mocks
5. Identify files involved
6. Propose minimal safe fix
7. Identify risks
8. Suggest validation steps
9. Decide routing:
   - DEBUG FIX mode
   - OR return to VERIFY FIX MODE

Output

Observed issue
- [concise]

Root cause
- [concise]

Non-blocking noise
- [concise]

Failure category
- [...]

Files involved
- [...]

Fix
- [...]

Why this fix
- [...]

Risks
- [...]

Validation
- [...]

Routing
- DEBUG FIX mode OR VERIFY FIX MODE

Rules
- no large rewrites
- prefer smallest fix
- stay within step scope
- state assumptions clearly

--------------------------------------------------

B. Debug Fix / Apply Minimal Fix

## When to use
Use only after DEBUG mode A identifies the fix.

## Inputs required
- feature slug
- current plan step
- docs/plans/<feature_slug>_plan.md
- agents.md
- architecture.md
- repo_map.md
- DEBUG findings

## Output expected
- minimal safe fix
- files changed
- reasoning
- validation notes

EXECUTE (DEBUG FIX)

You are a Staff Software Engineer applying the fix identified in DEBUG.

Task
Implement the minimal safe fix.

Rules
- stay within current step
- no scope expansion
- no unrelated changes
- follow repo patterns
- minimal, reversible changes

Output
1. Files changed
2. Fix applied
3. Why it works
4. Risks
5. Validation steps

Completion confirmation
- stayed within scope
- no unrelated changes