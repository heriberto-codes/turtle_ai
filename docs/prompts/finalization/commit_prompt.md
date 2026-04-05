/commit_prompt

## When to use
Use after the feature is complete and documented, when preparing the final git commit message.

## Inputs required
- feature slug
- `docs/plans/<feature_slug>_plan.md`
- `docs/features/<feature_slug>.md`
- `docs/backlog.md`
- modified source files

## Output expected
- commit message in `type(scope): short summary` format
- short commit body
- list of main files changed
- no git commands executed

You are preparing a git commit for the completed feature.

Review the changes related to this feature, including:

- docs/plans/<feature_slug>_plan.md
- docs/features/<feature_slug>.md
- docs/backlog.md
- any modified source files

Generate:

1. A concise git commit message following this format:
type(scope): short summary

Examples:
feat(players): add player analytics service
fix(auth): correct session rehydration bug
refactor(api): extract service layer from routes

2. A short commit body describing:
- the feature implemented
- key architectural decisions
- important files modified

3. A list of the main files changed.
Output only:
- the commit message
- the commit body
- the main files changed
Do not run git commands.