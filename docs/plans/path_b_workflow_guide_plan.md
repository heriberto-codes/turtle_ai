# Give Path B Its Own Workflow Guide

Feature slug: `path_b_workflow_guide`

Selected backlog item: extract Path B planning instructions into a separate workflow guide

## Architecture Grounding

Turtle is a Markdown-based workflow protocol implemented through repository-scoped Codex skills. Public documentation introduces the workflow; skill contracts and the four shared system documents define its behavior. This change reorganizes reader guidance without changing workflow contracts or artifact ownership.

## Approved Design Grounding

No matching design workspace exists. The user approved a defined documentation change: extract only Path B's instructions and leave Path A unchanged. No mockups or design approval artifacts are required.

## Tech Stack

Markdown files and relative links rendered by GitHub or a local Markdown viewer. No application runtime, package build, or automated repository test suite exists.

## Files Impacted

- `docs/guides/planning_workflow.md`: new public guide for Path B.
- `README.md`: compact Path B entry and Documentation list link.
- `architecture.md`: public guide directory and role.
- `repo_map.md`: public guide navigation and responsibility.

## Data / DB Changes

None.

## API Endpoints

None.

## Frontend Components

None. Markdown presentation and navigation only.

## Implementation Approach

Create the standalone guide first, then link it from the README, then align the foundation references. Preserve every instruction extracted from Path B, including the existing example and design approval caveat. Keep public guides separate from the four shared system documents copied during installation. Keep the README's shared implementation-loop transition sentence and preserve Path A exactly.

## Risks & Edge Cases

- Relative paths in the nested guide must resolve from `docs/guides/`.
- The guide must define feature slugs without relying on Path A.
- Choosing the non-design path must not bypass approval for an existing matching design workspace.
- Do not accidentally change installation requirements or duplicate Path B's detailed instructions in the README.

## Security Concerns

No runtime or security boundary changes. Use repository-relative links and do not introduce scripts, dependencies, or external data.

## Testing Strategy

Use documentation review, relative link and heading-anchor checks, and Markdown previews. Compare moved content against the original README, confirm Path A and installation content remain unchanged, and run `git diff --check`. No new test framework or permanent test harness is needed. Perform validation for each active step only, after its engineer checkpoint passes or the explicit task waiver applies.

## Assumptions and Unresolved Questions

- The user chose to shorten only Path B.
- The guide is titled "Plan Work Without the Design Phase" and lives at `docs/guides/planning_workflow.md`.
- Preserve the existing `contact_form` plan-path example.
- Add a return link to workflow selection, a forward link to the shared implementation loop, and links to relevant system references.
- Each step remains unchecked through EXECUTE, VERIFY, ENGINEER CHECKPOINT, and TEST. Only PLAN STEP UPDATE marks it complete.
- Task exception (2026-10-25): the user explicitly waived engineer checkpoints for this documentation update. Record the gate as waived, not PASS; retain implementation review, documentation validation, and controlled step updates. Repository-wide workflow rules remain unchanged.
- No unresolved product or implementation decisions remain.

## Step-by-Step Plan

- [x] Create `docs/guides/planning_workflow.md` with Path B's eligibility description, ideate/backlog instructions, existing-backlog shortcut, planning selection behavior, plan-review requirement, filename convention, and existing `contact_form` example. Define feature slugs, preserve the matching-design approval requirement, and add relative links back to workflow selection, forward to the shared implementation loop, and to relevant system references.
- [x] Update `README.md`: preserve Path B's heading and introductory sentence, replace its detailed instructions with a "Read the Planning Workflow Guide" link, add the guide to the Documentation list, and retain the shared implementation-loop transition sentence while moving its design-approval caveat into the guide. Preserve Path A and unrelated content.
- [x] Update `architecture.md` and `repo_map.md` to identify `docs/guides/` and its public navigation role, keeping the guide distinct from the four shared system documents required during installation.
