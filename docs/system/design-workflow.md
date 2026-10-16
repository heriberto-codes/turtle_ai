# Turtle AI Design Workflow

The design workflow is an optional pre-planning layer for user-facing features. It turns an existing interface and a product goal into an explicitly approved design direction before implementation planning begins.

```text
DESIGN AUDIT -> SECTION MOCKUPS -> PAGE ASSEMBLY -> DESIGN CHECKPOINT -> BACKLOG -> PLANNING
```

The workflow preserves Turtle AI's ownership principle: AI may inspect, propose, and visualize, but the user decides which direction may move into planning.

## When It Applies

Use the design workflow when a feature materially changes a page, screen, user journey, visual system, or interaction pattern.

Skip it for work with no meaningful design decision, such as backend-only changes, internal refactors, dependency maintenance, or a small copy correction with an already-defined result.

## Design Workspace

All artifacts for a design feature live under:

```text
docs/design/<feature_slug>/
```

`feature_slug` uses the same lowercase snake_case convention as Turtle plan files.

```text
docs/design/<feature_slug>/
|-- workflow-state.md
|-- audit.md
|-- design-spec.md
|-- checkpoint.md
|-- approved-design.md
`-- mockups/
    |-- index.html
    |-- styles.css
    `-- previews/
```

Only files useful to the actual design should be created. A mockup may keep its styles inside `index.html` when a separate stylesheet adds no value. Preview images are optional when the environment cannot render them; the editable HTML mockup remains the required visual artifact.

## Artifact Ownership

| Artifact | Write authority | Purpose |
| --- | --- | --- |
| `workflow-state.md` | `turtle-design-next` | Current automated design stage, confirmed section order, selections, and handoff status |
| `audit.md` | `turtle-design-audit` | Evidence-based assessment of the current experience and design constraints |
| `design-spec.md` | `turtle-design-mockup` | Implementable description of the proposed directions and design decisions |
| `mockups/*` | `turtle-design-mockup` | Isolated, editable visual concepts and optional rendered previews |
| `checkpoint.md` | `turtle-design-checkpoint` | Review status, user feedback, decisions, and requested revisions |
| `approved-design.md` | `turtle-design-checkpoint` | Explicitly approved source of truth handed to `turtle-plan` |

No design skill may modify production application code. Mockup code must remain inside `docs/design/<feature_slug>/mockups/` and must not be imported by the production application.

## Automated Mode

`/turtle-design-next` is the optional controller for this workflow. Each invocation discovers the active workspace, reads its state, performs exactly one safe transition through the relevant specialist contract, updates `workflow-state.md`, and stops.

Use the standalone audit, mockup, checkpoint, backlog, and plan commands when manual control is preferred. A standalone specialist may read workflow state for context but cannot update it.

The controller always pauses for user ownership decisions:

- confirmation of the audit's proposed ordered section list
- selection, combination, or revision of each section's mockup directions
- explicit approval of the complete assembled page

It may automate artifact discovery, section routing, page assembly, backlog creation, and planning. It never starts production implementation.

## Workflow State Contract

When automated mode is activated, its source of truth is:

```text
docs/design/<feature_slug>/workflow-state.md
```

Use Markdown with YAML frontmatter and a section table. Keep values machine-readable and decisions understandable to the user:

```yaml
---
feature_slug: example_feature
scope: page
status: awaiting_section_confirmation
sections_confirmed: false
current_section: null
assembly_status: pending
checkpoint_status: not_started
backlog_status: not_started
plan_status: not_started
backlog_item: null
plan_path: null
---
```

The body contains:

```text
# Design Workflow State

## Sections
| Order | Section | Status | Selected direction |
| --- | --- | --- | --- |
| 1 | hero | pending | — |

## Decisions
- Proposed section list awaiting confirmation.
```

Controlled workflow status values are:

- `audit_pending`
- `awaiting_section_confirmation`
- `section_mockup_pending`
- `section_selection_pending`
- `assembly_pending`
- `checkpoint_pending`
- `changes_requested`
- `approved`
- `backlog_pending`
- `plan_pending`
- `planned`

Controlled section status values are `pending`, `mockup_ready`, `selected`, and `revision_requested`. Assembly is `pending` or `complete`; checkpoint is `not_started`, `under_review`, `changes_requested`, or `approved`; backlog is `not_started` or `ready`; plan is `not_started` or `created`.

Only `/turtle-design-next` writes this file. It preserves confirmed order and completed selections across transitions. If an existing design folder is imported, the controller may derive state only from explicit artifact evidence. It must ask the user to confirm uncertain section matches or selections.

If more than one active design workspace exists and no slug is supplied, the controller lists the candidates and asks the user which one to continue. It never chooses by recency or filesystem order.

## Stage Contracts

### Design Audit

Inputs may include:

- user goal and intended audience
- current screenshots or a locally accessible page
- frontend code, components, styles, tokens, and assets
- existing brand rules and accessibility requirements

`audit.md` records:

- the user and business objective
- inspected evidence and missing context
- the current design inventory
- strengths to preserve
- usability, hierarchy, consistency, responsive, and accessibility findings
- constraints and reusable components or tokens
- prioritized opportunities for the mockup stage
- a proposed ordered page-section list for user confirmation in automated mode

Audit findings must be traceable to inspected evidence. An aesthetic preference must be identified as a recommendation, not presented as an objective defect.

### Mockup

The mockup stage requires `audit.md`. It creates at least one visual direction and normally creates two when the problem supports meaningful alternatives:

- a conservative direction emphasizing continuity and reuse
- a more ambitious direction exploring a stronger change

The primary artifact is responsive HTML/CSS under `mockups/`. It should be isolated from the production application, use realistic copy, and demonstrate the important page hierarchy and interaction states. Reuse recognizable project tokens or assets when available without altering their source files.

`design-spec.md` records for each direction:

- intent and rationale
- page structure and hierarchy
- components and assets reused or proposed
- typography, color, spacing, and other relevant tokens
- responsive behavior
- important interaction and content states
- accessibility considerations
- assumptions and known implementation implications

The mockup is a design artifact, not production-ready code. It must not claim functional behavior that the visual does not demonstrate.

In automated mode, mockups are created one confirmed section at a time. The user selects, combines, or requests revisions before the controller advances to the next section. After all sections have an explicit selection, the mockup contract assembles them into one complete responsive page. Intermediate sections must not recommend the final checkpoint.

### Design Checkpoint

The checkpoint reviews one meaningful decision at a time. It compares the mockup and specification against the audit, then records the user's answers in `checkpoint.md`.

When automated mode is active, final checkpoint review is blocked until every confirmed section has an explicit selected direction and the complete page assembly is recorded as complete.

The checkpoint establishes:

1. whether the proposed hierarchy serves the stated goal
2. which visual direction is selected
3. what must be preserved or revised
4. whether desktop and mobile behavior are acceptable
5. whether the user explicitly approves the design for planning

When changes are requested, set the checkpoint status to `changes_requested`, record actionable feedback, do not create `approved-design.md`, and route the work back to `turtle-design-mockup`.

Only after explicit user approval may the checkpoint set its status to `approved` and create `approved-design.md`.

## Approved Design Contract

`approved-design.md` is concise and contains:

- approval status and date
- selected direction
- approved goals and page structure
- required components, tokens, assets, and responsive behavior
- accepted tradeoffs or unresolved implementation questions
- links to the relevant audit, specification, and mockup files

Approval authorizes planning only. It does not authorize production implementation, plan-state changes, or backlog completion.

## Planning Handoff

An approved design is a valid feature source for `turtle-backlog`. The backlog skill creates a concise verb-first item or reuses an equivalent existing item; it must not duplicate the feature.

When a matching design workspace exists, `turtle-plan` requires `checkpoint.md` and `approved-design.md` to prove approval. It uses the approved design as the source of truth for design decisions. Repository code and `architecture.md` continue to control technical architecture and implementation constraints. Any conflict is recorded in the plan rather than silently resolved.

The automated workflow stops after creating `docs/plans/<feature_slug>_plan.md`. Production implementation begins only through an explicit later `/turtle-execute` invocation.

## Invariants

- Audit does not create a solution.
- Mockup does not approve itself or change production code.
- Checkpoint does not redesign the mockup.
- Only the checkpoint creates `approved-design.md`.
- Only the controller creates or updates `workflow-state.md`.
- Each controller invocation performs at most one transition.
- Section selection and final approval always require explicit user input.
- Design approval is not implementation approval.
- Automated design work stops after planning and never modifies production code.
