# Plan Work Without the Design Phase

[Back to Choose Your Workflow](../../README.md#choose-your-workflow)

Use this path for backend work, internal refactors, maintenance, or other work that needs no design exploration.

## Choose a Backlog Item and Create a Plan

A **backlog** is the list of features you might build, stored in `docs/backlog.md`. A **plan** breaks one selected feature into small implementation steps. Enter skill requests in Codex chat, one at a time.

1. If the feature is not already in the backlog, use `$turtle-ideate` to develop a grounded idea, then `$turtle-backlog` to save it. An existing suitable backlog item can be used directly.
2. Run `$turtle-plan` with the selected backlog item. If you omit a selection, it chooses the first unchecked backlog item.
3. Review the resulting plan before starting implementation.

## Plan Files

A **feature slug** is a short identifier used in filenames: `contact_form` uses lowercase words separated by underscores. Replace `<feature_slug>` in example paths with that identifier.

The plan is saved as `docs/plans/<feature_slug>_plan.md`. For example, a feature using `contact_form` has the path `docs/plans/contact_form_plan.md`.

## Existing Design Work

If a matching design workspace exists, planning requires its explicit approval even when planning is invoked directly. Choosing this path does not bypass that requirement. See the [Design Workflow](../system/design-workflow.md#planning-handoff) for the approval and planning handoff rules.

## Continue to Implementation

After reviewing the plan, continue to [Build One Plan Step at a Time](../../README.md#build-one-plan-step-at-a-time). Both workflow paths use the same implementation loop.

See the [Workflow Reference](../system/workflow.md) for the full phase sequence and the [State System](../system/state.md) for plan-step completion rules.
