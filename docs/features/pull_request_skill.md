# Optional GitHub Pull Request Skill

## Summary

Added `turtle-pull-request` as an optional, separately invoked finalization skill after push. The default creates a PR ready for review; `draft` uses the same completion gates; `preview` prepares text and a proposed destination without remote writes. Direct-push workflows remain supported.

## Architecture Impact

- Added the Markdown-only [skill contract](../../.agents/skills/turtle-pull-request/SKILL.md), with no runtime, dependency, or additional Turtle state system.
- Git owns commits and published branches; GitHub owns PR state. PR creation cannot change implementation, backlog, design, or feature-record completion, and does not imply approval, passing CI, or merge.
- Updated shared workflow/state documents, the engineering guide, architecture, and navigation together. The implementation loop and engineer checkpoint remain unchanged.
- Updated public installation guidance and the catalog to 25 skills, with ready/draft/preview examples. The overview retains five implementation steps and two state systems.

## Decisions

- Resolve published head repository/branch independently from target repository/base branch, including existing forks and differently named upstream branches. Use explicit targets or conventions, otherwise the target repository's default branch; clarify unresolved ambiguity.
- Creation requires completed applicable gates, a clean attached branch, and fresh published-head evidence matching reviewed local HEAD. Inspect the full commit range and merge-base diff. Preview may describe committed unpublished work and unfinished gates, clearly labeled, excluding uncommitted changes.
- Follow the target repository's PR template when present and ground descriptions in actual changes and observed test evidence.
- Return an exact matching open PR unchanged. Recheck reviewed state and duplicates immediately before creating once, then read back and verify the result. An uncertain response permits read-only reconciliation, never an automatic second creation attempt.
- Prefer sufficient connected GitHub tools, with authenticated CLI/API fallback and explicit destinations. Preview never uses `gh pr create --dry-run`; creation must prevent implicit pushes or forks.
- `turtle-commit` routes PR text requests to PR preview. `turtle-push` may recommend the optional skill after a verified push or synchronized result. Neither automatically invokes it.
- PR review begins on an appropriate feature branch before implementation. The skill never creates or switches branches, stages, commits, pushes, modifies existing PRs, merges, assigns reviewers, or publishes releases.
- Preserved pre-existing edits, including current push authorization for main/master. Historical feature records were not rewritten for this feature.

## Test Impact

All three [plan steps](../plans/pull_request_skill_plan.md) completed EXECUTE, VERIFY, ENGINEER CHECKPOINT, TEST, and PLAN STEP UPDATE. Each checkpoint returned PASS; step 1 included successful multiple-choice recovery. No permanent test framework or live test PR was introduced.

- Skill-creator validation passed for the new skill and changed commit/push contracts. Metadata checks confirmed 25 readable skill files with names matching their folders and a matching README catalog.
- Disposable local Git repositories exercised separate fork/base identities, differently named published branches, fresh head equality, a complete multi-commit comparison, dirty and unpublished work, empty comparisons, and detached HEAD detection.
- Simulated GitHub responses and instruction walkthroughs covered ready/draft/preview, exact open PR reuse without mutation, wrong-repository/closed PR exclusions, unresolved targets, unfinished gates, unrelated work, templates, missing test evidence, authentication failure, concurrent branch changes, and uncertain creation responses without retries. These validate the procedure and underlying mechanics; they are not end-to-end execution of an autonomous skill against GitHub.
- Local Markdown links and anchors resolved; the README check covered 22 local links/image references. Counts and all three invocation examples matched the skill contract. `git diff --check` passed.
- The initial generated illustration passed visual review but failed pixel preservation. DEBUG routed the correction to the active implementation step. With explicit user authorization, a deterministic edit copied the generated numeral into the original PNG. Read-only PNG checks confirmed valid chunks/CRCs, RGB format, and original 1254 × 1254 dimensions. All 10,076 changed pixels are confined to rectangle `(360,458)`–`(460,559)`; every pixel outside it matches the Git original. Final visual review passed.
- Scoped Turtle security and performance reviews found no concrete issues. The skill uses existing authenticated tooling, sanitized identities, explicit targets, safe PR-body handling, and bounded creation attempts; no application runtime path was added.

## Deployment Notes

Installation remains project-local copying of all 25 skill folders and the four shared system documents from one source revision. Optional GitHub PR inspection/creation requires sufficient connected tools or authenticated CLI/API access. This feature work did not stage, commit, push, or create a live PR.

## Follow-ups / Accepted Risks

Turtle remains an instruction-based protocol. Execution depends on the invoking agent and available tooling; the scenario reviews do not mechanically guarantee compliance. Gate evidence remains partly conversational, and branch tips cannot be atomically locked during PR creation; the skill requires fresh checks and reports readback mismatches without automatic repair.
