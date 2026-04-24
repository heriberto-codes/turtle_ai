---
name: turtle-test
description: Use this to determine and implement the minimal, correct set of tests for the current active plan step, ensuring behavior validation without modifying feature logic or expanding scope.
---

## Always read
- agents.md
- architecture.md
- repo_map.md

## External documentation rule
- If test design or implementation depends on third-party framework, test runner, library, or SDK behavior, use Context7 to consult current official documentation before making assumptions.
- Prefer repository code as the source of truth for local behavior and architecture.
- If repo code and external docs conflict, call out the conflict explicitly and favor the repo for existing implementation intent.

Modes:
- A = Determine required test coverage for the current active plan step under validation
- B = Implement the tests identified in TEST mode A

--------------------------------------------------

## A. Test Review / Test Planning

### When to use
Use after ENGINEER CHECKPOINT to determine the smallest correct set of tests needed for the current active plan step under validation.

### Inputs
- feature slug
- docs/plans/<feature_slug>_plan.md
- implemented code
- existing test files and patterns
- agents.md

### Output
- test coverage summary
- tests to update now
- tests to add now
- optional tests later
- edge cases
- regression risks
- exact file paths for test work

## Before testing
- read docs/plans/<feature_slug>_plan.md
- identify the FIRST unchecked step
- review implemented code for that step
- review existing test patterns
- do not modify plan state

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

## Output Format

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
- do NOT invent behavior; base tests on actual implementation
- prefer existing test utilities and helpers

Goal:
Identify the smallest correct set of tests needed to validate the current active plan step under validation.

Be concise.

--------------------------------------------------

## B. Test Follow-Up / Test Implementation

### When to use
Use only after TEST mode A identifies the exact tests to add or update.

### Inputs
- feature slug
- docs/plans/<feature_slug>_plan.md
- TEST mode A output
- existing test files and patterns
- relevant implementation files

### Output
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

## Before writing tests
- read docs/plans/<feature_slug>_plan.md and detect current step
- review implementation from last EXECUTE
- review TEST mode A output
- review existing test structure
- do not modify plan state

Tests to implement
<paste tests from TEST mode A here>

## Guard (CRITICAL)
- If "Tests to implement" section is empty → STOP:
  "Missing TEST mode A output. Cannot safely proceed."

- If BOTH "Tests to update now" AND "Tests to add now" are empty → STOP:
  "No actionable tests found from TEST mode A. Nothing to implement."

## Stop Conditions
- All identified tests implemented
- OR feature logic must change → STOP
- OR test expectations unclear → STOP

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
- do NOT invent tests beyond TEST mode A scope
- prefer existing test utilities and helpers

## Output Format
1. Tests added or updated
2. Files changed
3. Test coverage notes
4. Blockers / follow-ups

Completion confirmation:
- Confirm only tests were changed
- Confirm no unrelated feature logic was modified

Goal:
Add the smallest correct set of tests needed to validate the current plan step.
