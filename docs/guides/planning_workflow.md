# Engineering Workflow

[Back to Choose Your Workflow](../../README.md#choose-your-workflow)

Use this path for backend work, internal refactors, maintenance, or other work that needs no design exploration.

The implementation, recovery, and finalization instructions below apply to both workflow paths.

## Getting Started

Complete [installation](../../README.md#install-turtle-in-your-project) and [foundation setup](../../README.md#set-up-your-project-foundation) in your own project before using this guide. Enter skill requests in Codex chat, one at a time.

If you are starting work without design exploration, begin with [Choose a Backlog Item and Create a Plan](#choose-a-backlog-item-and-create-a-plan).

If the [Design Workflow](../system/design-workflow.md) has already created your plan, do not repeat backlog creation or planning. Review that plan, then continue to [Build One Plan Step at a Time](#build-one-plan-step-at-a-time). You can use the same entry point for any existing plan; if implementation has already started, first [resume from its recorded state](#resume-existing-work).

## Choose a Backlog Item and Create a Plan

A **backlog** is the list of features you might build, stored in `docs/backlog.md`. A **plan** breaks one selected feature into small implementation steps.

1. If the feature is not already in the backlog, use `$turtle-ideate` to develop a grounded idea, then `$turtle-backlog` to save it. An existing suitable backlog item can be used directly.
2. Run `$turtle-plan` with the selected backlog item. If you omit a selection, it chooses the first unchecked backlog item.
3. Review the resulting plan before starting implementation.

If no unchecked backlog item exists, planning stops. Add an appropriate item through the backlog phase before requesting a new plan.

### Plan Files

A **feature slug** is a short identifier used in filenames: `contact_form` uses lowercase words separated by underscores. Replace `<feature_slug>` in example paths with that identifier.

The plan is saved as `docs/plans/<feature_slug>_plan.md`. For example, a feature using `contact_form` has the path `docs/plans/contact_form_plan.md`.

### Existing Design Work

If a matching design workspace exists, planning requires its explicit approval even when planning is invoked directly. Choosing this path does not bypass that requirement. See the [Design Workflow](../system/design-workflow.md#planning-handoff) for the approval and planning handoff rules.

## Build One Plan Step at a Time

A **plan step** is one small checkbox item in the feature's plan. The first unchecked `[ ]` item is the active step. Run each skill below separately for the same feature, and continue only when its required result is satisfied:

| Order | Enter in Codex chat | What happens |
| --- | --- | --- |
| 1 | `$turtle-execute` | Implements only the active step. |
| 2 | `$turtle-verify` | Reviews correctness, scope, and alignment with the plan; requires PASS. |
| 3 | `$turtle-engineer-checkpoint` | Checks your understanding through a short conversation; requires PASS. |
| 4 | `$turtle-test` | Reviews needed coverage, then implements identified tests in a follow-up invocation when needed. Run the relevant project checks and inspect their results. |
| 5 | `$turtle-plan-step-update` | Checks completion evidence, then marks only that step `[x]` and identifies the next one. |

```text
EXECUTE -> VERIFY -> ENGINEER CHECKPOINT -> TEST -> PLAN STEP UPDATE
```

For example, enter `$turtle-execute feature_slug: contact_form` to identify the feature. Keep using that slug for its later phases, especially when more than one feature has a plan.

`$turtle-test` starts with **Mode A**, a review of the tests needed for this step. If it identifies tests to add or update, invoke it again with that review output and request **Mode B** to implement those tests. If no changes are needed, Mode B is unnecessary; record the applicable validation results before updating the step.

The checkbox stays unchecked throughout implementation and validation. Only `$turtle-plan-step-update` marks it complete after the required gates pass. Repeat the loop for the next unchecked step until the plan has none left.

Implementation stops if the active step requires work outside its scope or conflicts with `architecture.md`. Update the plan to resolve the issue before continuing; do not silently expand the work.

The engineer checkpoint asks at most three questions, one at a time, about the actual change. A material gap allows one guided retry, then one multiple-choice recovery. An unresolved recovery stops the checkpoint; TEST begins only after a PASS. See the [Engineer Checkpoint skill](../../.agents/skills/turtle-engineer-checkpoint/SKILL.md) for the full interaction rules.

## When Something Fails

Stop advancing the step. If the cause is unclear, use `$turtle-debug` in **Mode A** with the full failure output to diagnose it. Follow its recommended repair route:

| Finding | Next action |
| --- | --- |
| Incorrect code or a mismatch with the plan | Apply the scoped fix through the recommended EXECUTE debug/verify-fix mode. |
| Incorrect or missing tests | Return to TEST review and the identified test implementation work. |
| A misunderstanding at the checkpoint | Work through the checkpoint's explanation and recovery questions; investigate code if the misunderstanding exposes an implementation problem. |

DEBUG is conditional recovery, not a required stop after every successful test. Code fixes return through VERIFY, ENGINEER CHECKPOINT, and TEST; test fixes require fresh applicable validation results. Resolve failed gates and retain explicit checkpoint PASS evidence before running PLAN STEP UPDATE. Never jump directly from a debug diagnosis or fix to marking the step complete.

See the [Workflow Reference](../system/workflow.md) for test/debug modes and the [State System](../system/state.md) for completion rules.

## Resume Existing Work

When you return in a new chat, name the feature slug and ask Codex to read `docs/plans/<feature_slug>_plan.md`, report the active step, and identify missing gate evidence without changing files. Resume the needed phase; an unchecked item alone does not prove earlier gates passed.

If the first unchecked step already appears implemented, `$turtle-execute` may report `already_satisfied` without editing code. This does not complete the step: VERIFY must review the existing implementation, the engineer checkpoint must PASS, and TEST must complete with applicable validation results before `$turtle-plan-step-update` can mark it `[x]`.

If no unchecked plan steps remain, proceed to [Finalize the Feature](#finalize-the-feature).

## Finalize the Feature

After all plan steps are complete:

1. Run `$turtle-security` to review security risks and `$turtle-performance` for relevant performance risks. Address findings through the appropriate workflow before finalizing.
2. Run `$turtle-backlog-update` to mark the matching backlog item complete.
3. Run `$turtle-document` to record the completed feature in `docs/features/<feature_slug>.md`.
4. Run `$turtle-commit` to prepare a concise commit message from the actual changes.

`$turtle-commit` prepares the message. It does not automatically stage, commit, push, open a pull request, or publish a release. Those are separate Git/GitHub actions you request or perform after reviewing the changes.

## If You Get Stuck

| Situation | What to do |
| --- | --- |
| Turtle skills are missing | Confirm you opened your project, copied each skill into `.agents/skills/<skill-name>/SKILL.md`, and started a fresh Codex session. Repeat the [installation check](../../README.md#check-your-installation). |
| A foundation file is missing | Run the missing foundation skill in [setup order](../../README.md#set-up-your-project-foundation). Do not copy Turtle's own blueprint into your project. |
| Turtle pauses for approval | Reply with the requested confirmation, selection, or revision. For design, include it with the next `$turtle-design-next` request; see the [Design Workflow](../system/design-workflow.md#automated-mode). |
| A review, checkpoint, or test fails | Keep the step unchecked and follow [failure recovery](#when-something-fails). |
| You return in a new chat | Follow [Resume Existing Work](#resume-existing-work) to identify the active step and missing gate evidence before continuing. |
| Design work is already in progress | Run `$turtle-design-next` with its slug to resume from recorded design state. If several workspaces exist, name the one you want. See the [Design Workflow](../system/design-workflow.md#automated-mode). |

## References

- [Workflow Reference](../system/workflow.md): the full phase sequence and test/debug modes.
- [State System](../system/state.md): active-step detection, reconciliation, and completion rules.
- [Global Rules](../system/rules.md): shared workflow invariants.
- [Choose Your Workflow](../../README.md#choose-your-workflow): return to the README.
