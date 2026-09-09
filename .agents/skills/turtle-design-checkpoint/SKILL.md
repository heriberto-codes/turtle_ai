---
name: turtle-design-checkpoint
description: Use this after Turtle mockups exist to review one design decision at a time, capture user feedback, and explicitly approve one direction for planning. Do not use it to create mockups, implement code, or approve without user confirmation.
---

## Always read

- `agents.md`
- `architecture.md`
- `repo_map.md`
- `docs/system/design-workflow.md`
- `docs/design/<feature_slug>/audit.md`
- `docs/design/<feature_slug>/design-spec.md`
- relevant files under `docs/design/<feature_slug>/mockups/`

Also read `docs/design/<feature_slug>/workflow-state.md` when it exists. Treat it as controller-owned and do not modify it.

If a foundation file is absent, rely on the design artifacts and repository evidence that are available. Do not invent missing constraints.

## Preconditions

The audit, design specification, and at least one editable mockup direction must exist.

If they do not, stop and report:

```text
Design checkpoint blocked. Run /turtle-design-mockup for this feature first.
```

When `workflow-state.md` exists, final approval also requires:

- the user-confirmed section list is present
- every confirmed section has status `selected` and an explicit selected direction
- `assembly_status` is `complete`

If any condition is missing, do not begin or approve the final checkpoint. Report:

```text
Design checkpoint blocked. Select every confirmed section and assemble the complete page first. Run /turtle-design-next to continue.
```

## Purpose

Confirm that the user understands and accepts the proposed design direction before it enters implementation planning. This is a design-ownership gate, not a test of design vocabulary.

## Interaction mode

- Present the available visual directions before beginning the checkpoint.
- Ask exactly one meaningful design question at a time.
- Use plain language and connect each question to visible evidence.
- Record resolved decisions and actionable feedback in `docs/design/<feature_slug>/checkpoint.md`.
- Do not ask questions whose answers are already explicit in the user's request or prior checkpoint responses.

Cover only the decisions relevant to the feature, including:

1. whether the hierarchy supports the stated user and business goal
2. which direction should move forward
3. what must be preserved or changed
4. whether important desktop and mobile behavior is acceptable
5. whether the selected direction is explicitly approved for planning

The final approval question must clearly state that approval authorizes `turtle-plan`, not implementation.

When workflow state exists, review the complete assembled page at the feature level. Section-level selection is handled before this checkpoint and must not be mistaken for final approval.

## Checkpoint state

Create or update:

```text
docs/design/<feature_slug>/checkpoint.md
```

Track:

- status: `under_review`, `changes_requested`, or `approved`
- directions reviewed
- selected direction, if any
- resolved decisions
- requested revisions
- accepted tradeoffs
- remaining questions

Preserve earlier decisions when resuming a checkpoint. Do not restart the interview unless the mockup changed in a way that invalidates them.

## Changes requested

When the user requests revisions:

- set status to `changes_requested`
- translate feedback into concise, observable revision requirements
- do not edit the mockup or design specification
- do not create `approved-design.md`
- route the feature back to `/turtle-design-mockup`

## Approval

Only after the user explicitly approves a named direction for planning:

- set checkpoint status to `approved`
- create `docs/design/<feature_slug>/approved-design.md`
- summarize the selected direction, approved goals and structure, required components, tokens, assets, responsive behavior, accepted tradeoffs, and unresolved implementation questions
- link to the audit, design specification, selected mockup, and checkpoint record
- include the approval date and state that approval authorizes planning only

Do not infer approval from positive feedback, direction selection, silence, or a request to continue reviewing.

## Boundaries

- Only modify `checkpoint.md` and, after explicit approval, `approved-design.md`.
- Do not edit mockups, design specifications, production code, backlog state, or plan state.
- Do not answer checkpoint questions on the user's behalf.
- Do not create approval for an unnamed or ambiguous direction.
- Do not invoke implementation as part of approval.
- Do not create or modify `workflow-state.md`.

## Completion

If changes are requested, report the revision requirements. Recommend `/turtle-design-next` when workflow state exists; otherwise recommend `/turtle-design-mockup`.

If approved, link the approval artifact. Recommend `/turtle-design-next` when workflow state exists so backlog creation and planning occur as separate safe transitions; otherwise recommend `/turtle-plan` for the same `feature_slug`.
