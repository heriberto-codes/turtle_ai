---
name: turtle-commit
description: Review staged changes, generate an accurate commit message, and create one Git commit for completed Turtle work. Use after turtle-add and finalization gates. An explicit message-only request prepares text without committing. Do not stage files, push, or modify feature logic.
---

# Commit Reviewed Changes

Own only committing: review the index, prepare an accurate message, create one ordinary commit, verify it, and stop. Invoking this skill authorizes that commit when scope is clear; do not request a second approval for the same action. Honor an explicit message-only request and platform permission requirements. Never stage files or push as part of this skill.

## Always read

- `AGENTS.md` (or legacy `agents.md` only when `AGENTS.md` is absent)
- `architecture.md`
- `repo_map.md`

Identify the requested feature from the user request and conversation. Read its plan, completed feature record, and `docs/backlog.md` when available. This skill follows the existing completion and finalization gates; it cannot pass or mark them complete. Stop if available evidence shows required gates are unfinished or failed. If artifacts are absent, use available context and state assumptions outside the commit message; do not invent completion evidence or require unrelated artifacts for a small maintenance change.

## Review the staged scope

1. Confirm the repository root and inspect status, staged summaries, and the complete relevant staged diff. Inspect unstaged status to distinguish staged content from later edits. Do not expose secrets, credential-bearing URLs, or unrelated file contents.
2. Stop for unresolved conflicts or an in-progress merge, rebase, cherry-pick, or revert. Do not repair the repository or bypass hooks.
3. If the index is empty, stop without creating an empty commit and direct the user to `$turtle-add`. Message-only mode may still prepare text as described below.
4. Compare every staged path and its content with the requested work. If unrelated or ambiguous staging is present, explain the scope conflict and ask how to resolve it. Do not commit it, unstage it, or silently exclude it using a path-limited commit.
5. Unrelated unstaged work may remain untouched. If a file has both staged and unstaged changes, ground the commit and message in its staged version only. Stop if the staged content is incomplete or unsafe to commit on its own.

## Prepare the message

- Subject: `type(scope): short summary`, imperative mood, at most 72 characters.
- Type: `feat`, `fix`, `refactor`, `chore`, `docs`, `test`, `perf`, `build`, or `ci`.
- Choose the smallest accurate scope and summarize the actual staged behavior change.
- Use a short body for relevant implementation decisions and effects; avoid repeating the subject.
- Base claims on the staged diff and supporting evidence. Do not describe unstaged changes as committed, invent architecture changes, or claim tests passed without results for the staged content.
- Keep missing-context assumptions and validation limitations in the user-facing report, not in the commit message.

Show the proposed subject, body, and staged-file summary before executing. Clear invocation authorization is sufficient; ask only when the scope or an applicable repository rule remains unresolved.

## Create and verify one commit

- Record the current HEAD (or its absence for an initial commit), reviewed staged content, and staged-file list. Recheck them immediately before committing; if the review is stale, stop and inspect the changed state again.
- Create one ordinary commit from the current index. Supply the exact prepared message through safe structured arguments or a temporary message file; do not interpolate the message into shell code. Use no path-limited commit, automatic staging flag, empty-commit flag, amend, hook bypass, or signing bypass.
- Honor normal repository hooks and signing configuration. On rejection, stop and report the failure and actual Git state. Do not retry automatically, rewrite the commit, discard hook changes, or claim success solely because a command was attempted.
- After success, inspect HEAD, its parent(s), committed changes, and repository status. Confirm one commit was created from the intended prior HEAD and its contents match the reviewed staged version. Verify that unrelated unstaged work remains untouched. If hooks or concurrent changes produced an unexpected result, report it and stop without resetting or amending.

Return the commit ID, subject, committed-file summary, remaining working-folder changes, and any validation limitations. Identify `$turtle-push` as the next action when appropriate, but do not invoke it automatically. Never mark a plan or backlog item complete.

## Explicit message-only mode

When the user asks for only a commit message or a preview, inspect without changing Git state or creating a commit. Prefer staged content; if nothing is staged, use the requested unstaged/untracked work and clearly identify that source. If the user explicitly requests a message for unstaged work while other content is staged, keep those scopes separate and explain which source the draft describes.

Return:

1. Subject
2. Body
3. Files Changed

State missing-context assumptions separately and briefly. Do not include execution commands, stage files, commit, or push in this mode.

## Adjacent requests

- Staging belongs to `$turtle-add`; pushing existing commits belongs to `$turtle-push`. Route those actions to their owners without embedding or automatically running their procedures.
- PR text and creation belong to `$turtle-pull-request`; route PR-summary requests to `$turtle-pull-request preview` without drafting the PR here or automatically invoking that skill. Do not open a PR, publish a release, or push additional commits.
