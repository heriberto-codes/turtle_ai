---
name: turtle-push
description: Inspect the current branch, destination, and outgoing commits, then push existing commits for completed Turtle work. Use after turtle-commit. Invocation authorizes pushing the current branch, including main or master, after the usual checks. Require a clean working folder. Do not stage, commit, force-push, or create pull requests.
---

# Push Reviewed Commits

Own only publishing existing commits on the current branch. Invoking this skill authorizes an ordinary single-branch push when the scope and destination are clear, including when the local or destination branch is `main` or `master`. Honor preview-only requests and platform permission requirements. Report the intended action, perform it once, verify the result, and stop; never automatically invoke another skill.

## Always read

- `AGENTS.md` (or legacy `agents.md` only when `AGENTS.md` is absent)
- `architecture.md`
- `repo_map.md`

Use the request, conversation, and available feature artifacts to ground the intended work. This skill follows existing completion and finalization gates and cannot pass or mark them complete. Stop if available evidence shows required gates are unfinished or failed. Do not invent missing evidence or require unrelated feature artifacts for a small maintenance change.

## Inspect the repository

1. Confirm the repository root, current branch, HEAD, and status, including staged, unstaged, and untracked changes. Require a clean working folder and an attached branch with at least one commit. Stop for a dirty folder, detached HEAD, unresolved conflicts, or an in-progress merge, rebase, cherry-pick, or revert.
2. If files need staging or committing, report the blocker and route to `$turtle-add` or `$turtle-commit`; do not perform those actions here.
3. Inspect branch/upstream and remote push configuration without exposing credentials or credential-bearing URLs. Record the local branch and HEAD used for this invocation. Do not infer readiness from a previous chat or stale remote-tracking refs.

## Resolve one destination

- Prefer the current branch's configured upstream remote and destination branch, even when its name differs from the local branch. Require a named remote and a destination under `refs/heads/`.
- Without an upstream, use `origin` and the current branch name only when origin exists and the destination is unambiguous. A missing upstream branch on the remote is a first-push case, not proof of a completed earlier push.
- If the user's requested destination differs from the configured upstream, or configuration provides conflicting push remotes/refspecs, stop and clarify rather than silently choosing or changing repository configuration.
- Resolve the effective push endpoint as well as the fetch endpoint. Stop for multiple push endpoints, different fetch/push endpoints, local-dot upstreams, mirror configuration, or unsupported destinations; request a clear supported target. Do not treat fetch results from one repository as push evidence for another.
- Inspect fresh remote branch refs and fetch the branch objects needed for comparison using safe remote-tracking mappings. Do not update local branches, prune refs, merge, pull, or rebase. If remote evidence cannot be obtained, stop and report the access failure.

## Review outgoing work and authorization

- For an existing destination branch, compare its fresh tip with the recorded local HEAD. If equal, report already synchronized without pushing. If the remote is ahead or diverged, stop and report the mismatch; do not repair history. Otherwise list every commit that this push would add to that destination.
- For a new destination branch, list commits reachable from the recorded local HEAD but absent from all branch refs in that remote. If the remote has no branches, list all reachable commits. Explain that publishing a new branch is still an action even if its commits are already present on another remote branch.
- Show the local branch, sanitized remote identity, destination branch, and all outgoing commit IDs and subjects. Do not abbreviate the list in a way that hides older outgoing commits or assume the push contains only the latest commit.
- A bare `$turtle-push` authorizes publishing the current branch to the resolved destination after these checks, including when either branch is `main` or `master`. Do not require the user to repeat the invocation or name the branch for additional confirmation. This also applies when the configured upstream has a different branch name. If a requested branch name could mean switching branches or conflicts with the resolved destination, clarify first; never switch branches here.
- For preview-only requests, stop after reporting this review without publishing or changing upstream configuration.

## Push once and verify

- Recheck HEAD, the current branch, repository status, and destination configuration immediately before pushing. If they invalidate the review or authorization, stop and inspect again.
- Use an explicit single-branch refspec such as `refs/heads/<local>:refs/heads/<destination>`. Disable automatic tag following for this operation (`git -c push.followTags=false push ...`). Establish upstream with the successful first push when none was configured. Do not change an existing upstream silently.
- Use normal authentication, hooks, and remote protections. Never force-push (including force-with-lease), push tags, push all branches, mirror, delete remote branches, bypass hooks, or publish a release.
- On rejection or failure, stop and report the actual local/remote state. Do not retry automatically, create another commit, amend, pull, rebase, resolve conflicts, or change authentication configuration.
- After success, query the resolved remote destination and confirm its tip equals the reviewed local commit. Check local status and, for a first push, the resulting upstream. If verification is unavailable or reveals a mismatch, report that limit rather than claiming a verified push or retrying it.

Return a concise outcome: pushed, already synchronized, preview only, or stopped with reason. Include the branch, sanitized destination, published commit range, verification result, and any remaining user action. After a verified push or verified already-synchronized result, recommend optional `$turtle-pull-request` when the repository uses GitHub PR review and the published work belongs on a PR head branch. Direct-push workflows remain valid; preview, failure, and unverified results do not establish PR readiness. Never modify file contents or plan/backlog completion state, open a PR, or automatically chain another skill.
