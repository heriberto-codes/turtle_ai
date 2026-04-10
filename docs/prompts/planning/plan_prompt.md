/plan_prompt

## When to use
Use when planning a selected backlog item before implementation.

## Inputs required
- selected backlog item
- feature slug
- docs/backlog.md
- architecture.md
- repo_map.md
- agents.md
- repository code

## Output expected
- docs/plans/<feature_slug>_plan.md
- concise implementation plan
- atomic steps
- risks and testing strategy

Selected backlog item:
[paste exact item]

Feature slug:
[snake_case]

Save as:
docs/plans/<feature_slug>_plan.md

You are a Staff Software Engineer planning a feature.

Before planning:
- Inspect repository structure
- Review architecture.md, repo_map.md, agents.md
- Use these as the source of truth

Responsibilities
- prioritize maintainability
- minimize technical debt
- follow existing patterns

Planning requirements
1. Architecture overview
2. Tech stack detected
3. Files impacted
4. Database changes
5. API endpoints
6. Frontend components
7. Implementation approach
8. Risks and edge cases
9. Security concerns
10. Testing strategy
11. Step-by-step plan

Step rules
- atomic steps only
- each step independently implementable
- reference files
- small, reviewable diffs
- EVERY step MUST use markdown checkbox format: - [ ]
- Do NOT use numbered lists or plain bullets
- If any step is not in checkbox format, the plan is invalid
- order steps in the sequence they should be completed

Plan completion rule
Mark a plan step [x] only after it has passed:
- EXECUTE
- VERIFY
- ENGINEER CHECKPOINT
- TEST
- DEBUG (if needed)

Constraints
- no new frameworks
- no scope expansion
- align with repo patterns

Output style
- extremely concise
- the Step-by-step plan section must use markdown checkboxes

Include:
Assumptions
Unresolved questions

Validation rule:
- The Step-by-step plan must contain ONLY checklist items (- [ ])
- No numbered steps allowed

Do NOT write code.