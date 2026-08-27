# Turtle AI Design Workflow

The design workflow is an optional pre-planning layer for user-facing features. It turns an existing interface and a product goal into an explicitly approved design direction before implementation planning begins.

```text
DESIGN AUDIT -> MOCKUP -> DESIGN CHECKPOINT -> PLANNING
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
| `audit.md` | `turtle-design-audit` | Evidence-based assessment of the current experience and design constraints |
| `design-spec.md` | `turtle-mockup` | Implementable description of the proposed directions and design decisions |
| `mockups/*` | `turtle-mockup` | Isolated, editable visual concepts and optional rendered previews |
| `checkpoint.md` | `turtle-design-checkpoint` | Review status, user feedback, decisions, and requested revisions |
| `approved-design.md` | `turtle-design-checkpoint` | Explicitly approved source of truth handed to `turtle-plan` |

No design skill may modify production application code. Mockup code must remain inside `docs/design/<feature_slug>/mockups/` and must not be imported by the production application.

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

### Design Checkpoint

The checkpoint reviews one meaningful decision at a time. It compares the mockup and specification against the audit, then records the user's answers in `checkpoint.md`.

The checkpoint establishes:

1. whether the proposed hierarchy serves the stated goal
2. which visual direction is selected
3. what must be preserved or revised
4. whether desktop and mobile behavior are acceptable
5. whether the user explicitly approves the design for planning

When changes are requested, set the checkpoint status to `changes_requested`, record actionable feedback, do not create `approved-design.md`, and route the work back to `turtle-mockup`.

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

When `approved-design.md` exists, `turtle-plan` may use it as the source of truth for design decisions. Repository code and `architecture.md` continue to control technical architecture and implementation constraints. Any conflict should be called out in the plan rather than silently resolved.

Planning integration and enforcement are intentionally outside this MVP. They should be added only after the three design skills have been exercised against a real project.

## Invariants

- Audit does not create a solution.
- Mockup does not approve itself or change production code.
- Checkpoint does not redesign the mockup.
- Only the checkpoint creates `approved-design.md`.
- Design approval is not implementation approval.
