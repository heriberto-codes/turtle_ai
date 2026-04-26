---
name: turtle-commit
description: Use this when generating a final, concise git commit message for a completed feature, based on plans, feature docs, backlog updates, and modified files. Typically used after implementation, verification, and documentation are complete. Do not use for planning, implementation, debugging, or modifying code.
---

## Always read
- agents.md
- architecture.md
- repo_map.md

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
- optional follow-up prompts for commit, push, and PR summary

## Before writing
- verify all required docs exist; if any are missing, proceed with available context and note assumptions
- read plan, feature doc, backlog, and inspect modified files
- do not modify any files; this step is read-only

## Rules
- this phase generates a commit message first; do NOT modify code or run git commands automatically
- keep the subject line concise (≤ 72 characters)
- use imperative mood (e.g., "add", "fix", "refactor")
- choose an appropriate type: feat | fix | refactor | chore | docs | test | perf | build | ci
- scope should reflect the primary module or domain affected
- do NOT invent changes; base all statements on actual diffs and docs
- prefer the smallest accurate scope and summary
- avoid duplication between subject and body

## Task
Prepare a git commit message for the completed feature based on repository changes and documentation.

## Inputs
- feature_slug
- docs/plans/<feature_slug>_plan.md
- docs/features/<feature_slug>.md
- docs/backlog.md
- modified source files (git diff)

## Output
Return the following sections:

1) Subject
`type(scope): short summary`

2) Body
- what was implemented
- key architectural decisions
- notable files or modules affected

3) Files Changed
- list of main files (paths)


## Constraints
- do NOT include git commands in the initial commit message output
- do NOT include extra commentary
- keep output minimal and structured
- if context is incomplete, state assumptions briefly in the body

## Optional commit step
After generating the commit message, prompt the user with:

```text
Would you like Turtle AI to start the commit process using this message?
```

If the user says yes:
- show the exact git commands to run
- include `git status` before staging files
- include `git diff --stat` before committing
- use the generated commit message exactly unless the user asks to edit it
- do NOT execute git commands automatically
- do NOT modify any files
- require explicit user confirmation before any command execution

Suggested command sequence:

```bash
git status
git diff --stat
git add <files>
git commit -m "type(scope): short summary" -m "commit body"
```

If the user says no:
- stop after providing the commit message

## Optional push step
After the commit succeeds, prompt the user with:

```text
Would you like Turtle AI to push this commit to the current branch?
```

Before showing push commands:
- confirm the commit succeeded
- run or request `git status`
- confirm there are no uncommitted changes
- confirm the current branch name with `git branch --show-current`
- do NOT push from `main` or `master` without explicit confirmation
- do NOT force push
- do NOT push tags
- do NOT push all branches
- only suggest pushing the current branch
- show the exact command before doing anything
- require explicit user confirmation before any command execution

Suggested command sequence:

```bash
git status
git branch --show-current
git push origin <current_branch>
```

## Optional PR summary step
After the push step is complete or skipped, prompt the user with:

```text
Would you like Turtle AI to draft a PR summary for this feature?
```

If the user says yes, generate a concise PR summary using only the plan, feature doc, backlog update, and git diff context.

Use this format:

```markdown
## Summary
- ...

## Tests
- ...

## Notes for reviewer
- ...
```

Rules:
- do NOT invent changes
- do NOT claim tests passed unless evidence exists
- do NOT open a PR automatically
- do NOT push additional commits
- keep the PR summary concise and reviewer-friendly