# Add Turtle Git Finalization Skills

Feature slug: `git_finalization_skills`

Selected backlog item: add focused Git finalization skills for staging, committing, and pushing

## Architecture Grounding

Turtle is a Markdown-based workflow protocol. Skills own bounded operations; shared documents describe the contracts. Git operations use the adopting repository's Git installation and permissions, without introducing a Turtle runtime or new state system. Preserve the implementation loop and its comprehension gate.

## Approved Design Grounding

No matching design workspace exists. The user approved a defined skill-contract change; no UI exploration or design artifacts are required.

## Tech Stack

Markdown skills with YAML frontmatter, repository documentation, Git, and the existing PNG overview. Follow the existing SKILL.md-only folder convention; no helper scripts, UI metadata, dependencies, or application framework are needed.

## Files Impacted

- `.agents/skills/turtle-add/SKILL.md`
- `.agents/skills/turtle-commit/SKILL.md`
- `.agents/skills/turtle-push/SKILL.md`
- `docs/system/workflow.md`
- `docs/guides/planning_workflow.md`
- `README.md`
- `architecture.md`
- `repo_map.md`
- `assets/turtleAI.png`

## Data / DB Changes

None. Git's index, commits, and remote refs remain owned by Git. No additional workflow-state artifact is introduced.

## API Endpoints

None.

## Frontend Components

None. The existing documentation illustration needs only its skill count changed from 22 to 24.

## Implementation Approach

Use the sequence `$turtle-add` -> `$turtle-commit` -> `$turtle-push` after existing completion and finalization gates. An invocation authorizes only its named operation when scope is clear; report the proposed operation before acting and the observed outcome afterward. Do not automatically invoke the next skill. Platform permission prompts still apply.

ADD inspects staged, unstaged, and untracked changes; derives relevant paths from the user request, conversation, and available feature artifacts; and stages only explicit paths. Include relevant additions, edits, and deletions. Preserve unrelated staging and working-folder changes. Ask about unclear scope or mixed-purpose changes within a file instead of staging the whole file or selecting hunks silently. Stop on conflicting pre-staged work. Recheck the index and working folder immediately before staging and report the resulting staged scope. Do not commit, push, unstage, discard, or modify file contents.

COMMIT reviews the staged content and available plan, feature record, and backlog, generates a concise imperative `type(scope): summary` subject of at most 72 characters and an accurate body, and creates exactly one ordinary commit. Stage nothing. Stop for empty or unrelated staging and unresolved repository operations. Preserve unrelated unstaged edits; base the message on the index rather than unstaged work. Recheck the staged content before committing, use a safely supplied message, honor hooks, and verify success and resulting content before reporting it. An explicit message-only request performs read-only inspection and returns Subject, Body, and Files Changed without creating a commit. Note missing artifact assumptions outside the commit message without inventing completion evidence. Remove embedded staging/pushing procedures; route those requests to their owners. PR text may be drafted on explicit request, but no PR or release is created.

PUSH requires a clean working folder, attached branch, and no unresolved Git operation. Use the configured upstream's remote and destination branch; if absent, use origin and the same branch name only when unambiguous, establishing upstream on success. Stop for unsupported or conflicting remote configuration. Inspect the actual remote destination, refresh remote evidence, and show all outgoing commits before a single explicit branch push. Confirm pushes involving main/master unless the user's current authorization explicitly names that branch. Also protect a main/master remote destination reached from another local branch. Refuse behind/diverged history rather than pulling or rebasing; for a new branch list commits reachable from HEAD but absent from that remote's branch refs. Verify the destination ref matches the intended local commit after success. Already synchronized is a successful no-op. Never create commits, force push, push tags/all branches, or open PRs.

All three skills stop and report failures without expanding scope, bypassing hooks, amending, automatically pulling/rebasing, or resolving conflicts. None may update plan or backlog completion state. Do not print credentials in remote URLs or diagnostics.

## Risks & Edge Cases

- A bare turtle-commit intentionally changes from text preparation to commit creation; clearly document the migration and message-only alternative.
- Staging a mixed-purpose or partially staged file can include unrelated content; inspect both index and working-folder changes and ask when uncertain.
- Existing staging must be preserved; conflicting scope must not be silently replaced or committed.
- Upstream names can differ from local names, and main/master may be the destination even from a feature branch.
- Rejected hooks, authentication failures, protected branches, or concurrent changes must stop the operation without automatic repair.
- Skill counts become 24 only once both new skills exist; align references and illustration before finalization.

## Security Concerns

No new credentials or external services. Use explicit paths and destination refs, safe command argument handling, existing Git authentication, and ordinary hook execution. Avoid exposing secrets, credential-bearing URLs, or unrelated file contents. Testing must use disposable local repositories and a local bare remote, never test pushes to GitHub.

## Testing Strategy

After each step's VERIFY and engineer checkpoint PASS, use TEST review to select that step's validation. Validate changed skill definitions with the skill-creator quick validator and review observable decision paths, not exact wording. Use disposable repositories under the permitted temporary directory to exercise Git mechanics, recording which scenarios are instruction walkthroughs versus actual command execution. No permanent test framework is needed.

ADD coverage: relevant versus unrelated files; pre-staged unrelated work; conflicting staged scope; partial staging and mixed-purpose files; untracked additions; deletions; empty scope; preservation after failure.

COMMIT coverage: staged-only message grounding; empty/unrelated index; no stage/push side effects; message-only mode; missing artifacts; hook rejection; unchanged index checks and resulting commit contents.

PUSH coverage: feature-branch push; first push; upstream with a different branch name; synchronized no-op; dirty folder; detached branch; ambiguous destination; main/master authorization on either end; behind/diverged branches; rejected push with no automatic repair.

Documentation coverage: local Markdown links and anchors, 24 matching skill folders/frontmatter names, no stale message-only-default claims, and formatting checks. Visually verify the illustration still has its existing composition and text except the updated count.

## Assumptions and Unresolved Questions

- The user selected Add -> Commit -> Push, invocation authorization, and named-branch approval for main/master.
- Existing workflow gates remain required; no checkpoint waiver was requested for this feature.
- This implementation request does not itself authorize staging, committing, or pushing the unfinished feature.
- No unresolved product decisions remain.

## Step-by-Step Plan

- [x] Create `.agents/skills/turtle-add/SKILL.md` with scoped explicit-path staging, invocation authorization, staged/unstaged/untracked inspection, ambiguity and partial-staging safeguards, preservation of unrelated changes, failure handling, and staging-only output.
- [x] Update `.agents/skills/turtle-commit/SKILL.md` to review staged changes and create one commit by default, preserve message-only mode and documentation grounding, reject empty or unrelated staging, verify the commit, and route staging/pushing to their skill owners.
- [x] Create `.agents/skills/turtle-push/SKILL.md` with clean-folder and attached-branch requirements, upstream/origin destination resolution, outgoing-commit inspection, named approval for main/master, ordinary single-branch push, verified success/no-op results, and rejection handling without automatic repair.
- [x] Update `docs/system/workflow.md`, `docs/guides/planning_workflow.md`, `README.md`, `architecture.md`, and `repo_map.md` together to describe the ownership and authorization contract, the Add -> Commit -> Push sequence, the changed commit default and message-only alternative, both new skill locations, and the total of 24 skills.
- [x] Update only the skill count in `assets/turtleAI.png` from 22 to 24 while preserving its existing design, and visually review the resulting illustration.
