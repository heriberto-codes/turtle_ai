# Turtle AI Architecture

## Overview

Turtle AI is a repository-scoped, Markdown-based workflow protocol for AI-assisted software development. It runs through Codex skills rather than through an application server or executable runtime. Its purpose is to keep work small, reviewable, aligned with an explicit plan, and gated by engineer understanding.

The primary users are engineers who install or copy Turtle's skills into a software repository and then invoke the workflow through Codex. This repository is both the source distribution for those skills and the reference implementation of Turtle's workflow contracts.

## System Model

```text
Engineer request
      |
      v
Codex instruction loading -----> AGENTS.md
      |
      v
Skill routing ------------------> .agents/skills/<skill>/SKILL.md
      |                                  |
      |                                  v
      |                         docs/system/*.md
      |                                  |
      v                                  v
Repository inspection <---- architecture.md + repo_map.md
      |
      v
Owned Markdown artifacts and bounded repository changes
      |
      v
Next workflow gate or explicit stop
```

There is no long-running Turtle process. Each invocation loads the applicable instructions, derives current state from repository artifacts, performs one bounded responsibility, reports its result, and stops or routes to the next phase.

## Architectural Layers

| Layer | Location | Responsibility |
| --- | --- | --- |
| Repository guardrails | `AGENTS.md` | Concise, always-active rules, instruction hierarchy, safety boundaries, and workflow invariants. |
| Skill contracts | `.agents/skills/*/SKILL.md` | Procedures, inputs, outputs, ownership boundaries, stop conditions, and routing for the 25 Turtle skills. |
| Shared system contracts | `docs/system/` | Cross-skill workflow, state, design, and global rule definitions. |
| Foundation context | `architecture.md`, `repo_map.md` | Current system blueprint and repository navigation used by every Turtle phase. |
| Workflow artifacts | Generated in adopting repositories | Backlog, design evidence, plans, analysis, and completed feature records that carry state between invocations. |
| Public documentation | `README.md`, `docs/guides/`, `assets/` | Product introduction, quick start, workflow summary, reader guides, branding, and visual documentation. |

## Repository Structure

```text
.
|-- .agents/skills/       # 25 repo-scoped Codex skill contracts
|-- assets/               # README and brand imagery
|-- docs/guides/          # Public workflow guides
|-- docs/system/          # Shared workflow and state contracts
|-- AGENTS.md             # Always-active maintenance guardrails
|-- architecture.md       # This system blueprint
|-- repo_map.md           # High-level navigation map
|-- LICENSE               # MIT license and copyright notice
`-- README.md              # Public entry point
```

Turtle-generated feature artifacts such as `docs/backlog.md`, `docs/plans/`, `docs/design/`, `docs/analysis/`, and `docs/features/` are part of the downstream workflow contract. They are not required to exist in this source repository unless Turtle is actively using them for its own feature work.

`docs/guides/` contains public reader guidance, including the Engineering Workflow guide covering planning, implementation, failure recovery, resumption, and feature finalization. These guides are separate from the four shared system documents copied during installation; they do not add installation requirements or change skill contracts.

## System Boundaries

### Inside Turtle AI

- Repository-scoped skill definitions and their routing rules.
- Shared workflow, state, design, and safety documentation.
- File naming, artifact ownership, and transition contracts.
- Bounded staging, commit creation, single-branch push, and optional GitHub PR creation procedures carried out through Codex and repository tooling.
- Public usage documentation and brand assets.

### Outside Turtle AI

- Codex skill discovery, instruction loading, reasoning, and tool execution.
- Git storage, authentication, remote branch protections, and the host repository's branching, review, and release policies.
- GitHub PR storage, API/CLI availability, authentication, permissions, CI, approval, and merging.
- Application code, frameworks, databases, APIs, tests, and deployment systems in repositories that adopt Turtle.
- Optional rendering or visual-inspection tools used when producing design mockups.

Turtle does not provide an application runtime, API, database, authentication or authorization service, background worker, deployment target, package build, or telemetry service.

## Workflow Components

| Phase | Skills | Architectural role |
| --- | --- | --- |
| Foundation | `turtle-agents`, `turtle-architecture`, `turtle-repo-map` | Establish repository rules, system structure, and navigation context. |
| Discovery | `turtle-ideate`, `turtle-backlog`, `turtle-analyze` | Develop grounded understanding and persist candidate work. |
| Optional design | `turtle-design-next`, `turtle-design-audit`, `turtle-design-mockup`, `turtle-design-checkpoint` | Produce and approve isolated design evidence without changing production code. |
| Planning | `turtle-plan` | Convert one backlog item and any approved design into an atomic implementation plan. |
| Implementation | `turtle-execute`, `turtle-verify`, `turtle-engineer-checkpoint`, `turtle-test`, `turtle-debug`, `turtle-plan-step-update` | Implement one plan step through correctness, comprehension, testing, recovery, and controlled state transition. |
| Hardening | `turtle-security`, `turtle-performance` | Review completed behavior for security and performance risks. |
| Finalization | `turtle-backlog-update`, `turtle-document`, `turtle-add`, `turtle-commit`, `turtle-push`, `turtle-pull-request` | Close backlog state, record the completed feature, then stage reviewed changes, create a commit, push existing commits, and optionally open a GitHub PR through separate invocations. |

## Request and Control Flow

The top-level workflow is:

```text
FOUNDATION -> DISCOVERY -> OPTIONAL DESIGN -> PLANNING
           -> IMPLEMENTATION LOOP -> HARDENING -> FINALIZATION
```

For a typical skill invocation:

1. Codex loads `AGENTS.md` and selects the smallest applicable skill.
2. The skill reads `architecture.md`, `repo_map.md`, its required system contracts, and the current workflow artifacts.
3. Repository files remain the source of truth when documentation and tracked behavior conflict.
4. The skill derives state rather than asking for information already recorded in an owned artifact.
5. The skill performs only its declared responsibility and modifies only artifacts it owns.
6. The skill returns a bounded result, stop condition, or next valid route.

## State and Artifact Ownership

Turtle uses repository files as durable coordination boundaries.

### Design state

- Source of truth: `docs/design/<feature_slug>/workflow-state.md`.
- Sole writer: `turtle-design-next`.
- Specialist design skills may read controller state but may not edit it.
- Explicit user decisions are required for section confirmation, direction selection, requested revisions, and final design approval.
- Approved design may authorize backlog creation and planning, never production implementation.

### Implementation state

- Source of truth: `docs/plans/<feature_slug>_plan.md`.
- Active step: the first unchecked `[ ]` checklist item.
- Sole completion writer: `turtle-plan-step-update`.
- The active step remains unchecked through execute, verify, engineer checkpoint, test, and any debug cycle.
- No unchecked items means implementation is complete and may proceed to hardening and finalization.

### Git and PR finalization

Git remains the source of truth for staging, commits, and remote branch refs; no Turtle state file is added. `turtle-add` owns scoped index updates, `turtle-commit` owns one ordinary commit from the reviewed index, and `turtle-push` owns publishing existing commits to one resolved branch. They cannot change plan/backlog completion state or bypass unfinished gates.

`turtle-pull-request` owns PR text preparation and optional creation after push; it defaults to ready for review, supports explicit draft creation after the same gates, and returns exact matching open PRs unchanged. Preview is read-only and may describe committed unpublished work with outstanding gates labeled. Creation requires a clean attached branch and a published head matching reviewed local HEAD, with head and base resolved independently. GitHub owns PR state; no third Turtle state system is introduced. PR creation neither changes completion markers nor implies approval, passing CI, or merge. PR review workflows begin implementation on an appropriate feature branch; these finalization skills never create or switch branches.

The PR skill prefers sufficient connected GitHub tools, with authenticated CLI/API fallback and explicit targets preventing implicit pushes or forks. It never pushes, modifies existing PRs, merges, or enables auto-merge. COMMIT routes PR drafting to PR preview; PUSH may recommend PR creation after a verified push/synchronized result without invoking it. Direct-push workflows remain supported.

Each invocation authorizes only its named action when scope is clear. A bare `turtle-push` authorizes pushing the current branch to its resolved destination after the usual checks, including a local or destination main/master branch, without additional branch confirmation. Preview-only requests remain read-only, and platform permissions still apply. Ambiguous scope or configuration and rejected operations stop without automatic repair. A normal `turtle-commit` now creates a commit; an explicit message-only request preserves read-only drafting.

### Supporting artifacts

| Artifact | Owner or primary writer | Purpose |
| --- | --- | --- |
| `AGENTS.md` | `turtle-agents` | Repository conventions and safety constraints. |
| `architecture.md` | `turtle-architecture` | Current structural blueprint and constraints. |
| `repo_map.md` | `turtle-repo-map` | High-level navigation and protected paths. |
| `docs/backlog.md` | `turtle-backlog`, then `turtle-backlog-update` | Candidate and completed feature state. |
| `docs/design/<feature_slug>/` | Design controller and specialist skills according to contract | Audit, mockups, decisions, approval, and controller state. |
| `docs/plans/<feature_slug>_plan.md` | `turtle-plan`, then `turtle-plan-step-update` for checklist transitions | Ordered implementation steps and progress. |
| `docs/analysis/repo_analysis.md` | `turtle-analyze` when persistence is useful | Optional repository understanding. |
| `docs/features/<feature_slug>.md` | `turtle-document` | Final implementation record. |

## Architectural Principles

- **Comprehension before progress:** engineer understanding is a required gate, not an optional review.
- **One bounded responsibility:** each skill owns a narrow phase and must not bypass another skill's gate.
- **Explicit state:** durable Markdown artifacts carry design and implementation state between invocations.
- **Single-writer transitions:** controller and plan state have named owners to prevent conflicting updates.
- **Repository grounding:** tracked behavior overrides stale or incomplete documentation.
- **Minimal change:** skills preserve existing patterns, avoid scope expansion, and stop on unresolved conflicts.
- **Human approval:** design choices, comprehension, destructive operations, and ambiguous conflicts are not inferred.

## Security and Safety Model

Turtle's security model is procedural. `AGENTS.md` and individual skill boundaries prohibit exposing secrets, modifying protected or generated paths without authorization, silently resolving conflicting instruction files, or crossing workflow ownership boundaries. Security and performance skills review completed changes, while the host repository remains responsible for technical controls such as authentication, authorization, secret storage, dependency policy, and deployment security.

## Build, Run, and Deployment

Turtle has no compilation, package installation, application startup, environment configuration, or deployment process. It is activated when a compatible Codex environment opens the repository and discovers `.agents/skills/`. Validation consists of checking skill metadata and contracts, Markdown links, repository references, and Git diffs.

## Key Dependencies

- A Codex environment that supports repository-scoped skills and `AGENTS.md` instructions.
- A Git repository that stores skill definitions, system documentation, and generated workflow artifacts.
- Markdown-compatible tooling for reading and reviewing the contracts.
- Host-repository tools required by the software being planned, implemented, tested, or reviewed.
- Sufficient connected GitHub tools or authenticated GitHub CLI/API access only when optional PR inspection/creation is used; other workflows do not require GitHub.

## Known Risks

- **Contract drift:** duplicated concepts across skills and system documents can diverge without automated cross-contract checks.
- **Stale foundation context:** inaccurate architecture or navigation files can misdirect every downstream phase that reads them.
- **Limited automated validation:** this repository does not currently contain a repo-owned contract test harness or continuous-integration workflow.
- **Conversational gate evidence:** some verification, comprehension, and testing results exist primarily in the active conversation rather than durable per-step records.
- **Tool-dependent design verification:** preview rendering and visual inspection depend on capabilities available in the host environment.

These risks are documented here for visibility. Resolving them requires separately scoped workflow changes.

## Extension Points

- Add a new phase by defining a focused skill contract, its inputs and outputs, artifact ownership, routing, and corresponding shared documentation.
- Extend an existing phase by updating its owning `SKILL.md` and every affected shared contract in the same change.
- Add automated validation around frontmatter, links, cross-skill references, artifact ownership, and state transitions without changing the Markdown-first architecture.

## Maintenance Triggers

Refresh this document when any of the following changes:

- skill categories, names, or responsibilities
- workflow phase order or routing
- state source of truth or single-writer ownership
- generated artifact locations or schemas
- instruction hierarchy or repository boundaries
- build, validation, integration, or deployment characteristics

Refresh `repo_map.md` whenever tracked paths, entry points, or protected locations change.

## Open Questions

- What durable format should capture per-step gate evidence across Codex sessions?
- Which contract and scenario checks should become the repository's first automated validation suite?
- Should security and performance reviews gain explicit blocking verdicts and repair routing?
