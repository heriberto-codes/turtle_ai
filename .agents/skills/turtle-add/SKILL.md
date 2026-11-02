---
name: turtle-add
description: Stage reviewed changes relevant to the requested Turtle work using explicit file paths. Use before turtle-commit when files need staging. Do not create commits, push, discard changes, or update workflow completion state.
---

# Stage Relevant Changes

Own only staging: inspect the requested work, identify its files, update the Git index, report the result, and stop. Invoking this skill authorizes staging when scope is clear; do not request a second approval for the same action. Honor explicit preview-only requests and platform permission requirements.

## Always read

- `AGENTS.md` (or legacy `agents.md` only when `AGENTS.md` is absent)
- `architecture.md`
- `repo_map.md`

Use the request and conversation to identify the work. Read the matching plan, feature record, and backlog when available; do not invent missing artifacts or infer passed gates from implemented files. This skill follows the existing completion and finalization gates and cannot complete them itself.

## Inspect and select

1. Confirm the repository root and inspect repository status, including untracked files, staged changes, and unstaged changes. Stop for unresolved conflicts or an in-progress merge, rebase, cherry-pick, or revert; do not repair the repository.
2. Inspect staged and unstaged summaries and the relevant diffs. Examine relevant untracked files safely, respecting repository rules and protected paths. Do not expose secrets or unrelated file contents.
3. Derive an explicit list of repository-relative paths from the requested work and available evidence. Include relevant additions, modifications, and deletions. Do not treat every changed file as relevant or include ignored files by force.
4. Preserve pre-existing staging. Unrelated staged files must remain staged and be identified separately in the result; they are not authorization to commit them. If existing staging conflicts with the requested scope, stop and ask which scope is intended rather than unstaging or replacing content.
5. If a file contains both staged and unstaged changes, compare both before selecting it. Stage the whole file only when every change belongs to the requested work and replacing its staged version is clearly intended. For mixed-purpose edits, uncertain partial-staging intent, or unclear feature scope, explain the specific ambiguity and ask before changing the index. Do not silently stage the whole file or choose hunks.

Read-only Git inspection is allowed. Use status output that preserves unusual filenames (for example, porcelain output with NUL delimiters) when parsing paths programmatically. Quote paths safely; never interpolate file contents into shell commands.

## Stage

Before mutation, report the selected files and the intended staging operation. If the user has already authorized this scope, proceed without another confirmation.

- Recheck the index and working folder against the inspected state immediately before staging. If changes invalidate the reviewed selection, stop and inspect again rather than staging unseen edits.
- Use `git add -A -- <explicit-paths>` or an equivalent explicit-path operation to include reviewed deletions and additions. Pass literal paths safely, including paths with spaces or Git pathspec metacharacters. Do not use repository-wide `git add .`, `git add -A` without paths, or wildcard selections.
- Do not stage file contents containing secrets or files prohibited by repository rules. If relevant content cannot be inspected safely, report the blocker without exposing it.
- If the selected changes are already staged, report an already-staged no-op. If there are no relevant changes, report that nothing was staged.
- On failure, stop and report the observed result and any partial staging. Do not unstage existing content, retry with broader paths, or attempt automatic rollback.

## Verify and report

Inspect status and staged changes after the operation. Confirm that the selected staged content matches the review and that unrelated staged and unstaged changes were preserved. If an unexpected change appears, report it and stop without trying to reset the index.

Return a concise result:

- Outcome: staged, already staged, nothing to stage, or stopped with reason.
- Files staged for this request and a short staged-change summary.
- Any pre-existing unrelated staging and remaining unstaged or untracked work, without exposing sensitive contents.
- Next action: `$turtle-commit` when the staged scope is ready; if unrelated staging remains, explain that its scope must be resolved before committing.

Do not create or amend commits, push, invoke the next skill automatically, change file contents, discard or unstage changes, bypass hooks, or mark plan/backlog items complete.
