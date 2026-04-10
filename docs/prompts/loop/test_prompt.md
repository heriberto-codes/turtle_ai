/test_prompt

Modes:
- A = Determine required test coverage for the current active plan step under validation
- B = Implement the tests identified in TEST mode A

--------------------------------------------------

## A. Test Review / Test Planning

### When to use
Use after ENGINEER CHECKPOINT to determine the smallest correct set of tests needed for the current active plan step under validation.

### Inputs required
- feature slug
- docs/plans/<feature_slug>_plan.md
- implemented code
- existing test files and patterns
- agents.md

### Output expected
- test coverage summary
- tests to update now
- tests to add now
- optional tests later
- edge cases
- regression risks
- exact file paths for test work

TEST

You are a Senior QA Engineer and Software Engineer.

Task
Determine the tests required for the current active plan step under validation.

Fail-safe (CRITICAL):
- Before proceeding, scan docs/plans/<feature_slug>_plan.md
- If NO steps exist → STOP and report:
  "Plan file is empty or invalid. Cannot determine test scope."
- If NO unchecked steps exist → STOP and report:
  "No active plan step found. Testing phase is complete."

Current step detection:
- Use the FIRST unchecked step (- [ ])
- This is the step being validated with tests

Before testing:
1. Read docs/plans/<feature_slug>_plan.md
2. Identify the current active step under validation
3. Review implemented code for that step
4. Review existing test patterns
5. Review agents.md

Responsibilities
- verify expected behavior for this step
- identify missing test coverage
- identify edge cases and regression risks
- follow existing repo test conventions

Testing requirements
1. Confirm what behavior this step changed
2. Identify existing tests to update
3. Identify new tests required now
4. Cover happy path
5. Cover edge cases
6. Cover failure states relevant to this step

Output format

Test coverage summary
- [concise]

Tests to update now
- [test description]
  - File: [exact file path]

Tests to add now
- [test description]
  - File: [exact file path]
  - Type: unit / integration / e2e

Optional tests later
- [test description]

Edge cases
- [bullets]

Regression risks
- [bullets]

Rules
- stay scoped to the current active plan step under validation
- follow existing test patterns
- avoid unnecessary complexity
- only modify relevant tests
- always specify exact file paths
- if no test convention exists, recommend one standard convention

Goal:
Identify the smallest correct set of tests needed to validate the current active plan step under validation.

Be concise.

--------------------------------------------------

## B. Test Follow-Up / Test Implementation

### When to use
Use only after TEST mode A identifies the exact tests to add or update.

### Inputs required
- feature slug
- docs/plans/<feature_slug>_plan.md
- TEST mode A output
- existing test files and patterns
- relevant implementation files

### Output expected
- new or updated tests only
- modified test file paths
- coverage notes
- blockers or follow-ups
- confirmation that no unrelated feature logic was changed

EXECUTE (TEST FOLLOW-UP)

Use only after TEST mode A identifies the tests that should be added or updated.

Task
Implement ONLY the missing or failing tests identified in TEST mode A.

Fail-safe (CRITICAL):
- Before proceeding, scan docs/plans/<feature_slug>_plan.md
- Identify the FIRST unchecked step (- [ ])
- If no unchecked step exists → STOP:
  "No active step found. Testing phase is complete."

Context
- Feature: <feature_slug>
- Plan file: docs/plans/<feature_slug>_plan.md

Current step detection (REQUIRED):
- Read docs/plans/<feature_slug>_plan.md
- Identify the FIRST unchecked step (- [ ])
- This is the step under test
- Do NOT rely on manual input

Tests to implement
<paste tests from TEST mode A here>

Before writing tests:
- Read the current active plan step under validation carefully
- Review implementation from last EXECUTE
- Review existing test structure and naming conventions

Scope
- Only address the specific tests identified above

Rules
- Only implement or update tests
- Do NOT modify feature logic unless required to make code testable
- If feature logic must change → STOP and report it
- Follow existing test patterns and structure
- Place tests in correct existing files
- Do NOT introduce new frameworks
- Keep changes minimal
- Do NOT fix unrelated failing tests
- Do NOT expand beyond current active plan step under validation

If a test fails due to unclear implementation:
- clearly state issue
- suggest minimal fix
- do NOT implement feature fix

Stop Conditions:
- All identified tests implemented
- OR feature logic must change → STOP
- OR test expectations unclear → STOP

Output Requirements:
1. Tests added or updated
2. Files changed
3. Test coverage notes
4. Blockers / follow-ups

Completion confirmation:
- Confirm only tests were changed
- Confirm no unrelated feature logic was modified

Goal:
Add the smallest correct set of tests needed to validate the current plan step.