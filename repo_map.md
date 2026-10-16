# Turtle AI Repository Map

## Purpose

This map provides fast navigation for Turtle AI maintainers and for Turtle skills operating on this repository. It reflects tracked repository structure; planned or downstream-only artifacts are identified separately.

## Top-Level

| Path | Responsibility |
| --- | --- |
| `README.md` | Public entry point, version display, quick start, workflow summary, and documentation links. |
| `AGENTS.md` | Always-active repository maintenance rules and workflow guardrails. |
| `architecture.md` | Current system blueprint, boundaries, flows, ownership, risks, and extension points. |
| `repo_map.md` | This high-level navigation reference. |
| `.agents/skills/` | The 22 repo-scoped Codex skill contracts that implement Turtle's workflow. |
| `docs/system/` | Shared workflow, state, design, and global rule contracts. |
| `assets/` | README illustrations and Turtle AI logo files. |
| `.gitignore` | Ignores macOS `.DS_Store` metadata. |

## Primary Entry Points

| Goal | Start here |
| --- | --- |
| Understand Turtle AI | `README.md` |
| Apply repository-wide maintenance rules | `AGENTS.md` |
| Understand system structure and state ownership | `architecture.md` |
| Run or modify one workflow phase | `.agents/skills/<skill>/SKILL.md` |
| Understand cross-skill sequencing | `docs/system/workflow.md` |
| Understand implementation or design state | `docs/system/state.md` and `docs/system/design-workflow.md` |
| Understand global invariants | `docs/system/rules.md` |

## Skill Contracts

Every tracked skill lives at `.agents/skills/<skill-name>/SKILL.md`.

### Foundation

- `.agents/skills/turtle-agents/SKILL.md`
- `.agents/skills/turtle-architecture/SKILL.md`
- `.agents/skills/turtle-repo-map/SKILL.md`

### Discovery

- `.agents/skills/turtle-ideate/SKILL.md`
- `.agents/skills/turtle-backlog/SKILL.md`
- `.agents/skills/turtle-analyze/SKILL.md`

### Optional Design

- `.agents/skills/turtle-design-next/SKILL.md`
- `.agents/skills/turtle-design-audit/SKILL.md`
- `.agents/skills/turtle-design-mockup/SKILL.md`
- `.agents/skills/turtle-design-checkpoint/SKILL.md`

### Planning

- `.agents/skills/turtle-plan/SKILL.md`

### Implementation Loop

- `.agents/skills/turtle-execute/SKILL.md`
- `.agents/skills/turtle-verify/SKILL.md`
- `.agents/skills/turtle-engineer-checkpoint/SKILL.md`
- `.agents/skills/turtle-test/SKILL.md`
- `.agents/skills/turtle-debug/SKILL.md`
- `.agents/skills/turtle-plan-step-update/SKILL.md`

### Hardening

- `.agents/skills/turtle-security/SKILL.md`
- `.agents/skills/turtle-performance/SKILL.md`

### Finalization

- `.agents/skills/turtle-backlog-update/SKILL.md`
- `.agents/skills/turtle-document/SKILL.md`
- `.agents/skills/turtle-commit/SKILL.md`

## Shared System Documentation

| Path | Responsibility |
| --- | --- |
| `docs/system/workflow.md` | End-to-end phase order, bootstrap rules, skill roles, and recommended downstream project structure. |
| `docs/system/state.md` | Design-controller and plan-driven state sources, detection rules, flow, and reconciliation. |
| `docs/system/rules.md` | Cross-skill invariants for step state, debug routing, comprehension, and loop ordering. |
| `docs/system/design-workflow.md` | Design workspace schema, artifact ownership, controlled values, approvals, and planning handoff. |

## Public Assets

| Path | Responsibility |
| --- | --- |
| `assets/turtleAI.png` | README overview graphic. |
| `assets/Turtleai_logo/png/` | PNG logo variants, including the README header image. |
| `assets/Turtleai_logo/jpg/` | JPEG logo variants. |
| `assets/Turtleai_logo/svg/` | Vector logo variants. |
| `assets/Turtleai_logo/webp/` | WebP logo variants. |
| `assets/Turtleai_logo/pdf/` | PDF logo variants. |

## Repository-Owned vs. Generated Artifacts

The source repository owns the skill contracts, shared documentation, foundation documents, README, and assets listed above.

The following paths are workflow outputs created when a repository uses Turtle. They are documented contracts but are not permanent source modules in this repository:

| Generated path | Purpose |
| --- | --- |
| `docs/backlog.md` | Prioritized feature backlog and completion state. |
| `docs/analysis/repo_analysis.md` | Optional persisted repository analysis. |
| `docs/design/<feature_slug>/` | Audit, mockups, design specification, checkpoint, approval, and controller state. |
| `docs/plans/<feature_slug>_plan.md` | Ordered implementation plan and checklist state. |
| `docs/features/<feature_slug>.md` | Completed feature record. |

Do not treat an absent generated path as a missing Turtle source component unless the active workflow requires that artifact.

## Common Maintenance Routes

| Change | Primary location | Also review |
| --- | --- | --- |
| Repository-wide guardrail | `AGENTS.md` | Affected skills and system documents. |
| One skill's procedure or boundary | Its `.agents/skills/*/SKILL.md` | `docs/system/` when the contract is shared. |
| Workflow phase order or shared routing | `docs/system/workflow.md` | Affected skills, `architecture.md`, and README summary. |
| State source, values, or ownership | `docs/system/state.md` or `docs/system/design-workflow.md` | Owning skills, `docs/system/rules.md`, and `architecture.md`. |
| Plan-loop invariant | `docs/system/rules.md` | Implementation-loop skills and architecture. |
| Public usage or skill catalog | `README.md` | Confirm paths and version statements. |
| Brand or README imagery | `assets/` | README image references. |
| Repository structure | Tracked files and directories | Refresh `repo_map.md` and, when structural, `architecture.md`. |

## Protected and Special Paths

- `.git/` is repository metadata and must not be edited directly.
- `.agents/skills/*/SKILL.md` files are operational contracts; change them only when workflow behavior is intentionally in scope.
- `docs/system/` contains shared contracts; coordinate changes across every affected skill.
- `assets/` contains public branding; modify it only for an explicit branding or documentation change.
- Generated downstream artifacts have named owners. Do not modify controller state or plan checklist state outside the owning skill.
- `.DS_Store` files are ignored metadata and are not repository content.

## Notes

- There are no client, server, API, data, migration, build-output, or deployment directories in this repository.
- There is no repo-owned automated test or CI directory at present.
- Repository files are authoritative when this map or `architecture.md` becomes stale.
- Update this map when skills, system documents, entry points, tracked directories, or protected paths change.
