---
name: turtle-design-next
description: Use this to advance an activated Turtle design workflow by exactly one safe, state-aware transition. It discovers or resumes a design workspace, routes work through the design specialists, backlog, and planning, and stops before production implementation.
---

## Always read

- `agents.md`
- `architecture.md`
- `repo_map.md`
- `docs/system/design-workflow.md`

If a foundation file is absent, inspect the repository directly and record the missing context in the relevant specialist artifact instead of inventing it.

## Inputs

- `feature_slug` (optional)
- user or business objective when starting a new workspace
- user confirmation, selection, revision feedback, or approval supplied with this invocation

Normalize `feature_slug` to lowercase snake_case using only letters, numbers, and underscores.

## Purpose

Advance the design workflow by exactly one safe transition per invocation. Apply the relevant specialist skill contract for the current stage, then create or update only this controller-owned state file:

```text
docs/design/<feature_slug>/workflow-state.md
```

One transition may include creating the stage's specialist artifacts and reconciling the state file. Do not continue into the next transition during the same invocation.

## Workspace resolution

Resolve the design workspace before performing a transition:

1. Use an explicitly supplied `feature_slug` when present.
2. Otherwise, use the only active workspace containing `workflow-state.md` whose status is not `planned`.
3. Otherwise, use the only existing design folder that can be bootstrapped.
4. If multiple active or bootstrappable workspaces exist, stop, list their slugs and current stages, and ask which one to use.
5. If no workspace exists and the feature objective cannot be derived from the request, ask for the feature or page to design.

Never choose among multiple workspaces based on recency, folder order, or guesswork.

## State ownership and format

Only this controller may create or update `workflow-state.md`. Specialist skills may read it but must not edit it.

Use the schema defined in `docs/system/design-workflow.md`. Preserve the section-confirmation flag, confirmed section order, completed sections, selected directions, decisions, backlog reference, and plan path on every update.

Valid workflow statuses are:

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

## Existing workspace bootstrap

When a design folder exists without `workflow-state.md`:

- inspect `audit.md`, `design-spec.md`, `mockups/`, `checkpoint.md`, and `approved-design.md`
- create state from explicit artifact evidence
- propose an ordered section list from the audit and existing mockup structure
- require user confirmation of the section list before advancing
- treat a section as selected only when an artifact explicitly names its selected direction
- treat approval as valid only when both `checkpoint.md` records `approved` and `approved-design.md` exists
- mark uncertain selections as awaiting user confirmation; never infer them from filenames, visual similarity, or the existence of mockups

Bootstrapping and recording the proposed section list is one transition. Stop afterward.

## Transition router

Read only the specialist skill required for the current transition, then follow its contract.

### 1. Audit

Condition: no workspace exists, or status is `audit_pending`.

- Read `.agents/skills/turtle-design-audit/SKILL.md`.
- Create or update the audit through that contract.
- Record the audit's proposed ordered section list in state with every section `pending`.
- Set status to `awaiting_section_confirmation`.
- Ask the user to confirm, reorder, add, remove, or rename sections.
- Stop.

### 2. Section confirmation

Condition: status is `awaiting_section_confirmation`.

- If the user did not supply an explicit section confirmation or changes, show the proposed list and ask for confirmation. Do not change state.
- If the user explicitly confirms or edits the list, record the final order and section names.
- Set `sections_confirmed` to `true`.
- Preserve matching section work already supported by artifacts; require confirmation for ambiguous matches.
- Set the first unfinished section as `current_section` and status to `section_mockup_pending`.
- Stop without creating mockups.

### 3. Section mockup

Condition: status is `section_mockup_pending` or `changes_requested`.

- Choose the first section in confirmed order whose status is `pending` or `revision_requested`.
- Read `.agents/skills/turtle-design-mockup/SKILL.md`.
- Create or revise that section's isolated mockups and specification through the specialist contract.
- Preserve every completed section and its selected direction.
- Set the section status to `mockup_ready` and workflow status to `section_selection_pending`.
- Stop and ask the user to select, combine, or revise the available directions.

### 4. Section selection or revision

Condition: status is `section_selection_pending`.

- Never select a direction without an explicit user choice.
- If no clear choice or revision request is supplied, present the available directions for `current_section` and ask the user to select, combine, or revise them. Do not change state.
- If revisions are requested, record concise observable feedback, set the section to `revision_requested`, set workflow status to `section_mockup_pending`, and stop.
- If a direction or explicit combination is selected, record it and set the section status to `selected`.
- If another section remains unfinished, set it as `current_section`, set status to `section_mockup_pending`, and stop without creating its mockups.
- If all confirmed sections are selected, clear `current_section`, set status to `assembly_pending`, and stop.

### 5. Full-page assembly

Condition: status is `assembly_pending` and assembly is not complete.

- Read `.agents/skills/turtle-design-mockup/SKILL.md`.
- Assemble the selected sections into one complete responsive page inside the existing mockup workspace.
- Do not redesign selected sections except for necessary cross-section continuity; record any material conflict instead of silently changing an approved section decision.
- Update `design-spec.md` with assembly order, shared tokens, responsive transitions, and cross-section behavior.
- Set `assembly_status` to `complete` and workflow status to `checkpoint_pending`.
- Stop without beginning the checkpoint.

### 6. Feature-level design checkpoint

Condition: status is `checkpoint_pending` and all sections are selected.

- Read `.agents/skills/turtle-design-checkpoint/SKILL.md`.
- Follow its one-question-at-a-time checkpoint contract for the complete assembled page.
- Keep the workflow in `checkpoint_pending` while review questions remain.
- If the user requests changes, record the affected section and feedback, set that section to `revision_requested`, set `assembly_status` to `pending`, set workflow status to `changes_requested`, and stop.
- Set `current_section` to the affected section and `checkpoint_status` to `changes_requested`.
- Only after the user explicitly approves the named complete-page direction may the checkpoint create `approved-design.md`. Then set checkpoint and workflow status to `approved`.
- Stop. Approval does not create the backlog item in the same invocation.

### 7. Approved design to backlog

Condition: status is `approved` or `backlog_pending`.

- Verify `checkpoint.md` records `approved` and `approved-design.md` exists.
- Read `.agents/skills/turtle-backlog/SKILL.md`.
- Create or reuse the matching verb-first feature item from the approved design.
- Record the exact backlog item, set `backlog_status` to `ready`, and set workflow status to `plan_pending`.
- Stop without creating a plan.

### 8. Backlog to plan

Condition: status is `plan_pending` and backlog status is `ready`.

- Read `.agents/skills/turtle-plan/SKILL.md`.
- Pass the recorded backlog item and the exact `feature_slug` to the planning contract.
- Require the plan to use `approved-design.md` for design intent and repository architecture for technical decisions.
- Record the plan path, set `plan_status` to `created`, and set workflow status to `planned`.
- Stop before `/turtle-execute`.

### 9. Planned

Condition: status is `planned`.

- Make no file changes.
- Report the plan path and state that design automation is complete.
- Tell the user that implementation begins only when they explicitly run `/turtle-execute`.

## Revision routing

For `changes_requested`, identify the affected section from explicit checkpoint feedback. If more than one section could be responsible, ask the user which section to revise. Never overwrite a selected section based on ambiguous feedback.

After a revised section is explicitly selected, return to `assembly_pending` so the complete page is assembled and checked again before approval.

## Safety invariants

- Perform exactly one transition per invocation.
- Never infer section confirmation, design selection, revision acceptance, or final approval.
- Never modify production application code or production assets.
- Never import mockup code into the application.
- Never let design approval authorize implementation.
- Never run `/turtle-execute` or change plan checkboxes.
- Never modify specialist artifacts outside the active specialist contract.
- Never allow a specialist skill to write `workflow-state.md`.

## Completion

Report the workspace slug, transition performed, artifact paths created or updated, resulting workflow status, and the single user action or `/turtle-design-next` invocation needed to continue.
