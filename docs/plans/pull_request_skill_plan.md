# Add Turtle Pull Request Skill

Feature slug: `pull_request_skill`

Selected backlog item: add an optional GitHub pull request skill after push

## Architecture Grounding

Turtle is a Markdown-based workflow protocol with bounded skill ownership. Add an optional finalization skill after push; preserve the implementation loop, comprehension gate, direct-push workflows, and existing artifact owners. GitHub remains the source of truth for PR state; no Turtle runtime or third state system is introduced.

## Approved Design Grounding

No matching design workspace exists. The user explicitly approved the skill contract and requested implementation, backlog registration, and planning. Use that supplied feature as the backlog source; no additional ideation or UI design is needed.

## Tech Stack

Markdown skill instructions with YAML frontmatter, shared Markdown documentation, Git, GitHub connector or authenticated CLI/API, and the existing PNG overview. Follow the SKILL.md-only convention; no helper scripts, UI metadata, dependencies, or application framework.

## Files Impacted

- `.agents/skills/turtle-pull-request/SKILL.md`
- `.agents/skills/turtle-commit/SKILL.md`
- `.agents/skills/turtle-push/SKILL.md`
- `docs/system/workflow.md`
- `docs/system/state.md`
- `docs/guides/planning_workflow.md`
- `architecture.md`
- `repo_map.md`
- `README.md`
- `assets/turtleAI.png`

## Data / DB Changes

None. Git owns branch and commit state; GitHub owns PR state. Plan and backlog completion ownership remains unchanged.

## API Endpoints

No Turtle API. Use sufficient GitHub connector capabilities, or authenticated GitHub CLI/API fallback, for read-only repository/branch/PR inspection and one PR creation. No new credentials or authentication configuration.

## Frontend Components

None. Update only the overview illustration's skill count from 24 to 25 while preserving its existing design.

## Implementation Approach

Create `turtle-pull-request` with ready-for-review default, explicit draft mode after the same completion gates, and read-only preview. Creation requires completed applicable gates, a clean attached branch, and a published head matching local HEAD. Preview may describe committed unpublished work and excludes uncommitted changes.

Resolve the published head independently from the base repository/branch, including existing forks and differently named upstream branches. Respect explicit targets and repository conventions; otherwise use the target repository's default branch. Clarify ambiguity. Inspect the complete merge-base diff and proposed commit range, not only the last commit. Ground the title/body in that scope, the repository template, and observed validation evidence.

Return an existing exact matching open PR unchanged. Otherwise show the proposal, recheck reviewed state, create once, and verify the result. Invocation authorizes creation when clear; platform permission requirements still apply. Uncertain failures permit read-only reconciliation, never automatic creation retries. No staging, committing, pushing, branch/fork creation, metadata updates, merging, reviewer assignment, or releases.

Move PR drafting from COMMIT to PULL REQUEST preview. PUSH may recommend the optional next skill after verified push/synchronization without automatically invoking it. Update shared workflow, state explanation, guide, and foundation documents together; preserve current push authorization, including main/master. Document feature-branch preparation before implementation for PR workflows, without adding a branch-management skill.

Update public installation counts/catalog/examples and illustration to 25 skills. Do not change existing historical feature records. Preserve all pre-existing working-folder edits.

## Risks & Edge Cases

- Fork heads and base repositories differ; never assume origin is the PR target or the local branch name is the published name.
- CLI PR creation can implicitly push/fork; specify destinations explicitly, and use read-only inspection for preview rather than create --dry-run.
- Existing PR descriptions and draft state must not be overwritten; closed/merged PRs must not be automatically reopened.
- Concurrent changes or timeouts may invalidate reviewed state or hide a successful creation; inspect before reporting, never blindly retry.
- Missing gate/test evidence is not a passing result. Do not require unrelated feature artifacts for maintenance work.
- Documentation counts remain temporarily stale until their owning steps run; final validation must align all current references and imagery.

## Security Concerns

Use existing authenticated tooling, sanitized remote identities, explicit targets, and structured arguments or a temporary body file. Never expose credentials or interpolate PR text into shell code. No live PRs or pushes during validation.

## Testing Strategy

Run each step separately through EXECUTE, VERIFY, ENGINEER CHECKPOINT, TEST, and PLAN STEP UPDATE; keep the active checkbox unchecked until every gate passes. Apply DEBUG only when needed. Use the skill-creator validator and bounded scenario walkthroughs with disposable Git repositories and simulated GitHub responses; distinguish actual mechanics checks from instruction review. No permanent test framework is needed.

Cover ready/draft/preview; same-repository/fork/differently named published branches; exact duplicate reuse; dirty/detached/unpublished branches; ambiguous targets; identical or empty comparisons; unrelated commits; incomplete gates; templates/multi-commit summaries/missing test evidence; authentication failures; concurrent changes; and uncertain creation without duplicates. Check changed Markdown links, metadata, skill counts, preserved existing changes, and illustration appearance.

## Assumptions and Unresolved Questions

- The user selected GitHub only, optional finalization, ready-for-review default, exact existing PR reuse without updates, and moving PR drafting from commit.
- Their implementation request preserves the per-step engineer checkpoint; no waiver was requested.
- This request authorizes source and documentation changes, not staging, committing, pushing, or opening a live PR for this unfinished feature.
- Existing uncommitted edits in push and six documentation files must remain intact.
- No unresolved product decisions remain.

## Step-by-Step Plan

- [x] Create `.agents/skills/turtle-pull-request/SKILL.md` with creation/draft/preview behavior, independent head/base resolution, completion and published-head checks, full-diff drafting, duplicate reuse, bounded failure handling, verification, and PR-only ownership.
- [x] Update `.agents/skills/turtle-commit/SKILL.md`, `.agents/skills/turtle-push/SKILL.md`, `docs/system/workflow.md`, `docs/system/state.md`, `docs/guides/planning_workflow.md`, `architecture.md`, and `repo_map.md` together for PR drafting ownership, optional post-push routing, feature-branch guidance, GitHub-owned PR state/tooling requirements, the new skill location, and current foundation skill counts.
- [x] Update `README.md` installation guidance, catalog, counts, and ready/draft/preview examples to 25 skills, and change only the skill count in `assets/turtleAI.png` from 24 to 25 with visual verification.
