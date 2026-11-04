---
name: turtle-pull-request
description: Create a GitHub pull request for completed, pushed Turtle work, optionally after turtle-push. Default to ready for review; support explicit draft and read-only preview requests. Return an existing matching open PR unchanged. Do not push, merge, or change workflow completion state.
---

# Open a Reviewed Pull Request

Own only preparing PR text and creating one GitHub pull request from an existing published branch. This is an optional finalization step; direct-push workflows remain valid. Invoking this skill authorizes PR creation when scope and destination are clear, without a second approval for the same action. Honor preview-only requests and platform permissions. Report the proposal, perform the named action once, verify it, and stop without automatically invoking another skill.

## Always read

- `AGENTS.md` (or legacy `agents.md` only when `AGENTS.md` is absent)
- `architecture.md`
- `repo_map.md`
- `docs/system/workflow.md` and `docs/system/state.md`

Identify the intended work from the request and conversation. Read the matching plan, backlog item, completed feature record, and validation evidence when available. Creation follows existing implementation, hardening, and finalization gates; it cannot pass or mark them complete. Stop when required gates are unfinished or failed. Do not infer passed gates from code or checkboxes alone, invent missing evidence, or require unrelated feature artifacts for a small maintenance change. Draft creation requires the same gates as ready-for-review creation.

## Select the mode

| Request | Action |
| --- | --- |
| `$turtle-pull-request` | Create a PR ready for review. |
| `$turtle-pull-request draft` | Create a draft PR. |
| `$turtle-pull-request preview` or a PR-text-only request | Inspect and return proposed PR text and destination without creating or updating a PR. |

Preview may describe committed local changes before push; label unpublished work and outstanding gates. Exclude staged, unstaged, and untracked content from the proposed changes. Preview does not establish readiness or waive creation requirements.

## Inspect and resolve the comparison

1. Confirm the repository root, current branch, HEAD, and staged/unstaged/untracked status. Stop for detached HEAD, no commits, unresolved conflicts, or an in-progress merge, rebase, cherry-pick, or revert. Creation requires a clean working folder. Route necessary staging/committing to `$turtle-add`/`$turtle-commit`; do not perform them. Preview may inspect committed work in a dirty folder, explicitly excluding uncommitted changes.
2. Resolve the head repository and published branch from the current branch's upstream and effective push destination, or an unambiguous origin/current-branch destination without an upstream. The published branch name may differ from the local name. Inspect configuration without printing credentials or credential-bearing URLs. Clarify conflicting configuration or explicit requests; do not silently change upstreams, remotes, or the selected local branch.
3. Resolve the base repository separately. Respect explicit targets and applicable repository conventions; for an existing fork, use its verified GitHub parent when no other target is specified, otherwise the head repository. Resolve the base branch from the explicit request or unambiguous repository convention (including configured PR base); otherwise query the target repository's default branch. Never assume `main`, `master`, origin, or the local branch is the PR target. Ask when the target remains ambiguous. Support existing forks within GitHub's permitted repository network; stop for unsupported hosts or targets.
4. Obtain fresh GitHub/remote evidence for both repository identities and branch tips. For creation, require the published head to exist and exactly match the recorded local HEAD; route missing/unpublished commits to `$turtle-push`. Stop for ahead/diverged remote head or inaccessible evidence. Safe object fetching for comparison may update remote-tracking refs only; do not update local branches, prune, pull, merge, or rebase. Preview may compare local committed HEAD before publication and must label that source.
5. Compare the complete proposed commit range and merge-base diff against the resolved base, using equivalent complete GitHub comparison data when sufficient. Stop for the same repository/branch on both sides, no common history, no proposed commits or file changes, unrelated work, or a diff that cannot be fully inspected. Do not mistake equal branch names in different fork repositories for an identical head/base. Do not assume the latest commit is the whole PR. Keep secret values and unrelated private content out of reports and PR text.

## Draft from the reviewed scope

- Inspect applicable PR templates in the target repository at the base revision, including `.github/`, repository root, `docs/`, and named PR-template directories. Honor a clearly selected template; clarify multiple equally applicable templates instead of silently choosing. Do not create a template or rewrite local files.
- Follow the template when present; otherwise use Summary, Tests, and Notes for reviewer when useful. Lead with the concrete problem and resulting behavior. Summarize the whole proposed change, relevant decisions, and material limitations without pasting the plan or commit log.
- Ground test claims in observed validation for the reviewed content. Distinguish passed, failed, not run, and unavailable evidence; a missing result is not a pass. Leave unverified template checkboxes unchecked. Use verified repository URLs when linking artifacts; local absolute paths do not work as GitHub links.
- Do not add reviewers, assignees, labels, milestones, projects, issue-closing directives, or attachments as inferred side effects. Metadata management and review follow-up are outside this skill.
- Show the sanitized head/base identities and branches, reviewed head commit, proposed title/body, intended ready/draft state, and any validation limitations. Preview ends here with no remote writes or repository file changes; it must never call PR creation, including `gh pr create --dry-run`.

## Reuse or create once

1. Prefer sufficient connected GitHub tools; otherwise use authenticated GitHub CLI/API for the resolved host. Inspect available capabilities before acting. If required access or operations are unavailable, report the blocker rather than installing tools, changing authentication configuration, or assuming support.
2. Before creating, look for open PRs in the target repository for the exact head repository, published branch, and base branch. Inspect returned metadata to confirm repository identity, including forks; do not rely solely on branch text, recent-PR limits, or an incomplete search. If the search cannot establish the result, use a sufficient paginated listing or stop. One exact match means return its URL and observed status unchanged, even when its draft state or text differs from the current request. Multiple exact matches require clarification. Never overwrite, retarget, reopen, or convert an existing PR.
3. Immediately before creation, recheck local branch/HEAD/status, resolved targets, fresh remote head/base tips, and exact matching open PRs. If the reviewed state changed, stop and report the stale comparison; do not silently publish a different proposal. Creation cannot atomically lock branch tips; verification must catch later races.
4. Create once with explicit target repository, base branch, published head, title, body, and draft selection. For cross-repository requests use GitHub's qualified head and `head_repo` when required. Use structured connector/API arguments, or a safely written temporary body file for CLI `--body-file`; never interpolate PR text into shell code.
5. If using `gh pr create`, specify `--repo`, `--base`, `--head`, `--title`, and `--body-file`, adding `--draft` only for draft mode. Explicit `--head` prevents automatic pushing/forking. For fork shapes the CLI cannot represent (including unsupported organization-qualified heads), use sufficient connector/REST support or stop; do not relax target checks or fall back to implicit behavior.
6. On failure, stop without automatic retries or repair. If the result is uncertain, perform a read-only lookup for the exact PR and inspect any returned URL before reporting. Report a found PR and what can be verified; otherwise state that creation is unconfirmed. Never repeat a create call automatically, including through a different tool.

## Verify and report

Read the created PR back from GitHub. Verify its canonical URL/number, open status, head and base repository identities/branches, head commit matching the reviewed HEAD, title/body, and requested draft state. Recheck local status; confirm this skill did not stage, commit, push, or change local files. If a branch race or unavailable readback prevents confirmation, report the observed mismatch or verification limit without editing/deleting the PR or retrying creation. Ready for review does not mean approved, passing CI, or merged; report observed checks only when available without waiting for or rerunning them.

Return a concise outcome: created, existing PR, preview only, or stopped with reason. Include the PR link when known, head/base, ready/draft state, verification result, and remaining user action. GitHub owns PR state; never write a Turtle PR-state file, edit plan/backlog completion, or update the feature record as part of this skill.

Never stage, commit, push, create/switch/delete branches or forks, repair history, bypass hooks/protections, merge, enable auto-merge, publish releases, or automatically chain another skill. Route necessary Git actions to their owning skills; PR review and merging remain the repository's own process.

## Tool references

- [GitHub CLI PR creation](https://cli.github.com/manual/gh_pr_create): explicit head behavior, body files, draft mode, CLI fork limitations, and the side effects possible with dry-run.
- [GitHub pull request REST API](https://docs.github.com/en/rest/pulls/pulls): listing, qualified cross-repository heads, `head_repo`, creation, and readback.
