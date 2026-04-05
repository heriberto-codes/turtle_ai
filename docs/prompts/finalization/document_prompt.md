/document_prompt

## When to use
Use after the feature is fully complete, not after every step.

## Inputs required
- feature slug
- implemented feature changes
- `docs/plans/<feature_slug>_plan.md`
- changed files
- test impact
- deployment notes if any

## Output expected
- `docs/features/<feature_slug>.md`
- final feature record
- summary of what was built
- important decisions
- architecture impact
- test impact
- follow-ups and accepted risks

Create the final feature record for the implemented feature.

Purpose
- capture what was built
- record important implementation decisions
- preserve risks, tradeoffs, and follow-up context for future work

Include:
- feature summary
- architecture impact
- important decisions made
- API endpoints changed or added
- database changes (if any)
- test impact
- deployment considerations
- known follow-ups or accepted risks

Rules
- document only what was actually implemented
- be concise
- do not restate the full plan
- do not invent future work unless clearly identified as follow-up

If a new test structure was introduced, document the chosen pattern in agents.md or architecture.md so future steps follow it consistently.

Save as:
docs/features/<feature_slug>.md

If docs/features/ does not exist, create it.