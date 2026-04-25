<!-- #TODO make sure the plan document does not check off the backlog before I hit the /Turtle-backlog-update skill -->

---
name: turtle-plan
description: Use this to generate a concise, repo-aligned implementation plan for a selected backlog item, producing atomic, checkbox-based steps scoped to the current feature. Do not write code or modify plan state.
---

## Always read
- agents.md
- architecture.md
- repo_map.md

## External documentation rule
- If planning depends on third-party framework, library, or SDK behavior, use Context7 to consult current official documentation before making assumptions.
- Prefer repository code as the source of truth for local behavior and architecture.
- If repo code and external docs conflict, call out the conflict explicitly and favor the repo for existing implementation intent.

## Inputs
- selected_backlog_item
- feature_slug
- docs/backlog.md
- architecture.md
- repo_map.md
- agents.md
- repository code

## Before planning
- read agents.md, architecture.md, repo_map.md
- inspect repository structure and conventions
- map backlog item to existing modules and patterns
- do not modify plan state

## Output
- docs/plans/<feature_slug>_plan.md
- concise plan with atomic checkbox steps
- risks, assumptions, testing strategy

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
1. Architecture overview (concise)
2. Tech stack detected (from repo)
3. Files impacted (paths)
4. Data / DB changes (if any)
5. API endpoints (if any)
6. Frontend components (if any)
7. Implementation approach (safest path)
8. Risks & edge cases
9. Security concerns
10. Testing strategy
11. Step-by-step plan (checkboxes only)

## Step Rules
- atomic steps only
- each step independently implementable
- reference exact file paths
- small, reviewable diffs
- EVERY step MUST use markdown checkbox format: - [ ]
- no numbered lists or plain bullets
- order steps sequentially

## Plan completion rule
Mark a plan step [x] only after it has passed:
- EXECUTE
- VERIFY
- ENGINEER CHECKPOINT
- TEST
- DEBUG (if needed)
- this is the ONLY place where completion criteria is defined; do NOT mark steps here

## Constraints
- no new frameworks
- no scope expansion
- align with repo patterns
- do NOT write code

## Output Format
- concise sections per Plan Sections
- Step-by-step plan uses ONLY - [ ] items
- include Assumptions and Unresolved questions
- no code blocks

selected_backlog_item:
[paste exact item]

feature_slug:
[snake_case]

Save as:
docs/plans/<feature_slug>_plan.md
