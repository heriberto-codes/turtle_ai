---
name: turtle-plan
description: Use this to generate a concise, repo-aligned implementation plan for a selected backlog item, or the next unchecked backlog item when none is provided. Produces atomic, checkbox-based steps scoped to the current feature. Do NOT write code or modify backlog state.
---

## Always read
- agents.md
- architecture.md
- repo_map.md
- docs/backlog.md

## External documentation rule
- If planning depends on third-party framework, library, or SDK behavior, use Context7 to consult current official documentation before making assumptions.
- Prefer repository code as the source of truth for local behavior and architecture.
- If repo code and external docs conflict, call out the conflict explicitly and favor the repo for existing implementation intent.

## Architecture grounding rule
`architecture.md` is the primary source of truth for system structure.

Before generating a plan, extract and use the following from `architecture.md` when available:

- backend framework and architectural pattern
- frontend framework and routing pattern
- API structure and naming conventions
- data flow and state management approach
- database and persistence decisions
- authentication/session model
- testing approach
- deployment/runtime assumptions
- known constraints and protected boundaries

Planning must align with the architecture described in `architecture.md`.

If `architecture.md` does not contain enough information, inspect the repository and state the gap under Assumptions or Unresolved questions.

If repository code conflicts with `architecture.md`, do not guess. Call out the conflict explicitly in the plan and favor existing repository behavior for implementation safety.

## Inputs
- selected_backlog_item (optional; if omitted, use auto-selection)
- feature_slug (optional; if omitted, generate from selected backlog item)
- docs/backlog.md
- architecture.md
- repo_map.md
- agents.md
- repository code

## Backlog item selection
Manual selection is allowed and takes precedence:
- If `selected_backlog_item` is provided, use that exact backlog item.
- If `feature_slug` is provided, use that exact slug only if it is valid snake_case.
- If a provided `feature_slug` is not valid snake_case, convert it to valid snake_case before using it.

Auto-selection is the default when inputs are omitted:
- Read `docs/backlog.md`.
- Select the first unchecked backlog item that uses markdown checkbox format: `- [ ]`.
- Ignore checked items: `- [x]` or `- [X]`.
- Preserve the exact selected backlog item text for planning context.
- Do NOT modify `docs/backlog.md`.
- If no unchecked backlog item exists, stop and report that there is no backlog item to plan.

Feature slug generation:
- If `feature_slug` is omitted, derive it from the selected backlog item text.
- Remove the leading markdown checkbox marker.
- Remove markdown formatting, issue labels, priority markers, and trailing punctuation when practical.
- Convert to lowercase snake_case.
- Use only letters, numbers, and underscores.
- Collapse repeated underscores and trim leading/trailing underscores.
- Keep the slug concise but recognizable.

Snake case requirement:
- `feature_slug` MUST be snake_case.
- The plan file path MUST be `docs/plans/<feature_slug>_plan.md` using the snake_case slug.
- Never create a plan file with spaces, hyphens, camelCase, PascalCase, uppercase letters, or punctuation in the slug.
- If the backlog item title contains hyphens, slashes, punctuation, or mixed casing, normalize them before writing the file.

## Before planning
- read agents.md, architecture.md, repo_map.md, docs/backlog.md
- determine `selected_backlog_item` and `feature_slug` using manual inputs or auto-selection
- extract the current system architecture from `architecture.md`
- inspect repository structure and conventions
- compare repo structure against `architecture.md`
- map backlog item to existing modules and patterns
- use `architecture.md` to ground framework, routing, API, data, auth, testing, and deployment assumptions
- do NOT invent architectural patterns that are not present in `architecture.md` or the repository
- do NOT modify plan state
- do NOT modify docs/backlog.md
- do NOT mark any backlog item complete

## Output
- create or update docs/plans/<feature_slug>_plan.md
- concise plan with atomic checkbox steps
- include risks, assumptions, and testing strategy

You are a Staff Software Engineer planning a feature.

Before planning:
- Inspect repository structure
- Review architecture.md, repo_map.md, agents.md
- Use these as the source of truth

Responsibilities
- prioritize maintainability
- minimize technical debt
- follow existing patterns

## Plan Sections
1. Architecture grounding from `architecture.md` (concise)
2. Tech stack detected from `architecture.md` and confirmed against repo
3. Files impacted (paths)
4. Data / DB changes (if any)
5. API endpoints (if any)
6. Frontend components (if any)
7. Implementation approach (safest path)
8. Risks & edge cases
9. Security concerns
10. Testing strategy
11. Assumptions and unresolved questions
12. Step-by-step plan (checkboxes only)

## Step Rules
- atomic steps only
- each step independently implementable
- reference exact file paths
- small, reviewable diffs
- EVERY step MUST use markdown checkbox format: - [ ]
- ALL steps must start as unchecked: - [ ]
- NEVER generate - [x] steps in this skill
- no numbered lists or plain bullets inside step section
- order steps sequentially
- NEVER include backlog status changes as a plan step
- NEVER generate a step that edits `docs/backlog.md`
- NEVER generate a step that says to check off, mark complete, or update a backlog item
- `/turtle-backlog-update` is the ONLY allowed backlog completion mechanism and must NOT appear as a step inside the implementation plan
- NEVER include final documentation as a plan step
- NEVER generate a step that edits `docs/features/*`
- NEVER generate a step that says to document the feature, write final docs, or update feature documentation
- `/turtle-document` is the ONLY allowed final documentation mechanism and must NOT appear as a step inside the implementation plan

## Plan completion rule
Mark a plan step [x] ONLY after it has passed:
- EXECUTE
- VERIFY
- ENGINEER CHECKPOINT
- TEST
- DEBUG (if needed)

- This skill must NOT mark steps complete
- This skill must NOT modify docs/backlog.md
- Backlog completion is ONLY done via /turtle-backlog-update

## Constraints
- no new frameworks
- no scope expansion
- align with `architecture.md`
- align with existing repo patterns
- do NOT introduce new architectural patterns unless explicitly required by the backlog item and called out as a risk
- do NOT write code
- do NOT modify docs/backlog.md
- do NOT check off backlog items

## Backlog update guardrail
This skill creates a plan only.

It must never mark the backlog item complete.
It must never include backlog completion as a step in the generated plan.
It must never include `docs/backlog.md` edits in the step list.

Backlog updates are ONLY allowed after full completion of the Plan-Driven State System via:

```text
/turtle-backlog-update
```

If asked to modify docs/backlog.md, STOP and respond:

```text
Backlog update blocked. Run /turtle-backlog-update after the feature is complete.
```

## Documentation guardrail
This skill creates an implementation plan only.

It must never include final documentation as a generated plan step.
It must never include `docs/features/*` edits in the step list.

Final documentation is ONLY allowed after full completion of the Plan-Driven State System via:

```text
/turtle-document
```

If asked to include final documentation work in the plan, STOP and respond:

```text
Documentation step blocked. Run /turtle-document after the feature is complete.
```

## Output Format
- write directly to docs/plans/<feature_slug>_plan.md
- concise sections per Plan Sections
- Step-by-step plan uses ONLY - [ ] items
- include Assumptions and Unresolved questions
- no code blocks

selected_backlog_item:
[optional; paste exact item or omit to use first unchecked backlog item]

feature_slug:
[optional; snake_case or omit to auto-generate]

Save as:
docs/plans/<feature_slug>_plan.md
