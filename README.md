<p align="center">
  <img
    src="assets/Turtleai_logo/png/turtle-ai-logo-with-slogan-1024w.png"
    alt="Turtle AI — Build with AI. Keep understanding."
    width="720"
  />
</p>

[![wakatime](https://wakatime.com/badge/user/3a78d911-a08a-40c2-bf2c-782c4d20eb23/project/083c1137-37af-417a-8f7d-632880b2dc31.svg)](https://wakatime.com/badge/user/3a78d911-a08a-40c2-bf2c-782c4d20eb23/project/083c1137-37af-417a-8f7d-632880b2dc31)

# Turtle AI

**Version:** v1.1.0

> Build with AI without losing understanding.

Turtle AI is a collection of Codex **skills**—reusable instructions for specific tasks—and Markdown workflow rules that help you build software in small, reviewable steps. You add Turtle to your project's **repository** (the project folder and its Git history), then use its skills in Codex chat to plan, design, implement, and review changes.

You bring the project, development tools, and testing environment. Turtle provides the workflow: AI helps with the work, and you stay responsible for understanding the result.

## Why Understanding Is a Gate

It is easy to accept AI-generated code that works without knowing why it works. Turtle makes understanding a required **checkpoint**: a short conversation that checks whether you can explain the change before moving on.

For a contact form, that might mean explaining where validation happens, why an invalid email is rejected, and how the form connects to the rest of your app. If something is unclear, the checkpoint helps you work through it. You do not need to get every answer right on the first try.

The goal is to keep changes small enough to review, test, and understand so you can maintain your own project later.

## Before You Start

- Use a Codex environment that supports repository skills and `AGENTS.md` instructions.
- Have a project folder under Git version control. A new project can start with an empty repository; an existing project can keep its current files and tools.
- Have the tools needed to run and test your project, such as its language runtime or framework. Turtle itself has no package dependencies or application server to start.

Follow [installation](#install-turtle-in-your-project), [foundation setup](#set-up-your-project-foundation), and then [choose a workflow](#choose-your-workflow). The [skill catalog](#skill-catalog) is a reference once you know the path you need.

## Install Turtle in Your Project

1. Get a copy of this repository's current `main` branch. On [GitHub](https://github.com/heriberto-codes/turtle_ai), select **Code → Download ZIP** and extract it. Or run this in a terminal from a folder where you keep source repositories:

   ```bash
   git clone https://github.com/heriberto-codes/turtle_ai.git
   ```

2. Find `.agents/skills/` in the downloaded Turtle folder. Folders starting with a dot may be hidden: press **Command + Shift + .** in macOS Finder, or enable **View → Show → Hidden items** in Windows File Explorer.
3. In your own project folder, create `.agents/skills/` if needed. Copy all **22 `turtle-*` folders**, including their contents, into it. Keep any unrelated skills already there.
4. Create `docs/system/` in your project if needed. Copy these four files from Turtle's `docs/system/`: `workflow.md`, `design-workflow.md`, `state.md`, and `rules.md`. The design skills read these shared documents directly.
5. Preserve Turtle's license notice: create `docs/licenses/` and copy Turtle's root `LICENSE` into it as `turtle-ai-LICENSE.txt`. Keep your project's own license file as it is.
6. Open **your project folder** in Codex and start a fresh chat so it can discover the copied skills.

If a destination skill folder or system document already has the same name, compare it with the incoming copy before replacing anything. Preserve local customizations and resolve differences explicitly. For later Turtle updates, repeat this comparison using skills and system documents from the same source revision.

Your copied files should look like this; the other 20 skill folders are omitted from the picture:

```text
your-project/
|-- .agents/skills/
|   |-- turtle-agents/SKILL.md
|   |-- turtle-architecture/SKILL.md
|   `-- ...
`-- docs/
    |-- system/
    |   |-- workflow.md
    |   |-- design-workflow.md
    |   |-- state.md
    |   `-- rules.md
    `-- licenses/turtle-ai-LICENSE.txt
```

Paste this into Codex chat to check the installation without changing files:

```text
Check my Turtle AI installation without modifying files. Confirm that all
22 turtle-* skill folders have a readable SKILL.md with a matching name,
and report whether those skills are available in this Codex session.
Check for workflow.md, design-workflow.md, state.md, and rules.md under
docs/system/, plus docs/licenses/turtle-ai-LICENSE.txt. Report missing
files or name conflicts. Do not run the skills or generate foundation files yet.
```

### How to Invoke a Skill

Enter skill requests in **Codex chat**, one at a time. For example:

```text
$turtle-agents
```

These are chat requests, not terminal commands. Codex also documents `/skills` as a way to select a skill; see [OpenAI's invocation guidance](https://developers.openai.com/blog/eval-skills). You can add context after the skill name, including the feature you want to work on.

## Set Up Your Project Foundation

Run these skills in order, waiting for each to finish:

| Order | Enter in Codex chat | Creates or refreshes |
| --- | --- | --- |
| 1 | `$turtle-agents` | `AGENTS.md`: repository rules and safety constraints. |
| 2 | `$turtle-architecture` | `architecture.md`: the current system blueprint. |
| 3 | `$turtle-repo-map` | `repo_map.md`: a navigation guide to important files and boundaries. |

After creating or changing `AGENTS.md`, begin a fresh Codex chat in the project before continuing so the repository instructions are loaded. For a Turtle-managed project, ask `$turtle-agents` to include the plan-step order, comprehension gate, and state ownership rules from `docs/system/rules.md` in its concise guardrails.

The foundation skills preserve valid existing content and update it incrementally. `$turtle-agents` can run before the other two documents exist. It creates uppercase `AGENTS.md`; if only legacy lowercase `agents.md` exists, it migrates its valid content to the canonical filename. If both exist, it stops for you to reconcile them. Other Turtle skills use lowercase only as a temporary fallback when uppercase is absent.

The foundation documents in **this Turtle repository describe Turtle itself**. Generate your project's versions from its own files instead of copying Turtle's versions. In an empty project, record unknowns honestly; these skills do not choose or scaffold an application stack. Refresh the documents after initial scaffolding and whenever rules, architecture, or repository structure materially change.

## Choose Your Workflow

A **backlog** is the list of features you might build, stored in `docs/backlog.md`. A **plan** breaks one selected feature into small implementation steps. Turtle saves these and other **artifacts**—files produced by the workflow—inside your project.

If you need to understand existing code first, use `$turtle-analyze`. If you need grounded feature ideas, use `$turtle-ideate`. Analysis is useful when entering an unfamiliar repository or module; it is not required before every small change.

### Path A: Design a User-Facing Feature

Use this path when a feature changes a page, screen, user journey, or interaction and you want to explore design choices before implementation.

Start with a goal, for example:

```text
$turtle-design-next Design an accessible contact form for my portfolio.
Use feature_slug: contact_form.
```

A **feature slug** is a short identifier used in filenames: `contact_form` uses lowercase words separated by underscores. In examples such as `docs/design/<feature_slug>/`, replace `<feature_slug>` with that identifier.

Keep invoking `$turtle-design-next`, supplying the requested feedback or approval, to follow this path:

```text
AUDIT -> SECTION CONFIRMATION -> SECTION MOCKUPS AND SELECTIONS
      -> PAGE ASSEMBLY -> DESIGN CHECKPOINT -> BACKLOG -> PLAN
```

Each invocation performs one safe transition and stops. You confirm the sections, choose or revise mockups, and explicitly approve the complete design. A pause is expected: Turtle is waiting for your decision or the next invocation.

Artifacts live in `docs/design/contact_form/`, including isolated mockups and `workflow-state.md`, the file that records the current design stage. For a new interface, supply requirements or references; the audit records missing visual evidence instead of inventing an existing UI.

After approval, continue invoking the controller until it creates or reuses the backlog item and creates the plan. You do not need to run backlog and planning again. The controller stops before production implementation; design approval alone never authorizes application-code changes.

For manual control, the specialist skills are `$turtle-design-audit`, `$turtle-design-mockup`, and `$turtle-design-checkpoint`. See the [Design Workflow](docs/system/design-workflow.md) for their boundaries and recovery rules.

### Path B: Plan Work Without the Design Phase

Use this path for backend work, internal refactors, maintenance, or other work that needs no design exploration.

1. If the feature is not already in the backlog, use `$turtle-ideate` to develop a grounded idea, then `$turtle-backlog` to save it. An existing suitable backlog item can be used directly.
2. Run `$turtle-plan` with the selected backlog item. If you omit a selection, it chooses the first unchecked backlog item.
3. Review the resulting plan before starting implementation.

The plan is saved as `docs/plans/<feature_slug>_plan.md`. For example, a feature using `contact_form` has the path `docs/plans/contact_form_plan.md`.

Both paths now continue through the same implementation loop. If a matching design workspace exists, planning requires its explicit approval even when planning is invoked directly.

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

### When Something Fails

Stop advancing the step. If the cause is unclear, use `$turtle-debug` in **Mode A** with the full failure output to diagnose it. Follow its recommended repair route:

| Finding | Next action |
| --- | --- |
| Incorrect code or a mismatch with the plan | Apply the scoped fix through the recommended EXECUTE debug/verify-fix mode. |
| Incorrect or missing tests | Return to TEST review and the identified test implementation work. |
| A misunderstanding at the checkpoint | Work through the checkpoint's explanation and recovery questions; investigate code if the misunderstanding exposes an implementation problem. |

DEBUG is conditional recovery, not a required stop after every successful test. Code fixes return through VERIFY, ENGINEER CHECKPOINT, and TEST; test fixes require fresh applicable validation results. Resolve failed gates and retain explicit checkpoint PASS evidence before running PLAN STEP UPDATE. Never jump directly from a debug diagnosis or fix to marking the step complete.

See the [Workflow Reference](docs/system/workflow.md) for test/debug modes and the [State System](docs/system/state.md) for completion rules.

## Finalize the Feature

After all plan steps are complete:

1. Run `$turtle-security` to review security risks and `$turtle-performance` for relevant performance risks. Address findings through the appropriate workflow before finalizing.
2. Run `$turtle-backlog-update` to mark the matching backlog item complete.
3. Run `$turtle-document` to record the completed feature in `docs/features/<feature_slug>.md`.
4. Run `$turtle-commit` to prepare a concise commit message from the actual changes.

`$turtle-commit` prepares the message. It does not automatically commit, push, open a pull request, or publish a release. Those are separate Git/GitHub actions you request or perform after reviewing the changes.

## If You Get Stuck

| Situation | What to do |
| --- | --- |
| Turtle skills are missing | Confirm you opened your project, copied each skill into `.agents/skills/<skill-name>/SKILL.md`, and started a fresh Codex session. Repeat the installation check above. |
| A foundation file is missing | Run the missing foundation skill in setup order. Do not copy Turtle's own blueprint into your project. |
| Turtle pauses for approval | Reply with the requested confirmation, selection, or revision. For design, include it with the next `$turtle-design-next` request. |
| A review, checkpoint, or test fails | Keep the step unchecked and follow [failure recovery](#when-something-fails). |
| You return in a new chat | Name the feature slug and ask Codex to read the plan and report the active step and missing gate evidence without changing files. Resume the needed phase; an unchecked item alone does not prove earlier gates passed. |
| Design work is already in progress | Run `$turtle-design-next` with its slug to resume from recorded design state. If several workspaces exist, name the one you want. |

## What's Included

![Turtle AI overview: 22 skills, an optional design workflow, 5 implementation steps with debugging when needed, and 2 state systems.](assets/turtleAI.png)

- **22 skills** for focused development tasks.
- **Optional design workflow** to explore mockups and approve a direction before coding.
- **5-step implementation loop:** execute → verify → engineer checkpoint (understand the change) → test → plan step update. Debugging is conditional recovery, not an extra required step.
- **2 state systems:** design workflow progress and implementation-plan progress.

## Skill Catalog

Turtle includes 22 project-local skills in `.agents/skills/`. Their names describe focused responsibilities; you do not need to memorize the full catalog to begin.

| Category | Skills |
| --- | --- |
| Foundation | `turtle-agents`, `turtle-architecture`, `turtle-repo-map` |
| Discovery | `turtle-ideate`, `turtle-backlog`, `turtle-analyze` |
| Design (optional) | `turtle-design-next`, `turtle-design-audit`, `turtle-design-mockup`, `turtle-design-checkpoint` |
| Planning | `turtle-plan` |
| Implementation | `turtle-execute`, `turtle-verify`, `turtle-engineer-checkpoint`, `turtle-test`, `turtle-debug`, `turtle-plan-step-update` |
| Hardening | `turtle-security`, `turtle-performance` |
| Finalization | `turtle-backlog-update`, `turtle-document`, `turtle-commit` |

## Foundation and Workflow Files

Turtle's foundation and workflow files have distinct responsibilities:

| Location | Role |
| --- | --- |
| Root `AGENTS.md` | Concise repository guardrails loaded as Codex instructions. |
| Root `architecture.md` | Current system blueprint, boundaries, and ownership. |
| Root `repo_map.md` | Navigation guide to important files and protected paths. |
| `.agents/skills/*/SKILL.md` | Detailed procedure for one skill, loaded when that skill applies. |
| `docs/system/` | Shared workflow and state rules used across skills. |

Skill procedures define each phase's operational behavior. Repository files remain authoritative if architecture or navigation documentation becomes stale. Keep the foundation current as your project evolves.

## Current Limitations

Turtle is an instruction-based workflow carried out by Codex, with human review and file-based state. It does not mechanically enforce every gate or guarantee bug-free or secure code. Some gate evidence lives in the conversation, so a new chat may need to re-establish it. This source repository currently has no automated contract-test suite or CI workflow, and mockup previews depend on the tools available in your environment. See [architectural risks](architecture.md#known-risks) for the current maintenance concerns.

## Documentation

The README is the starting guide. These documents explain the system and its detailed workflow rules:

- [Architecture](architecture.md)
- [Repository Map](repo_map.md)
- [Workflow Reference](docs/system/workflow.md)
- [Design Workflow](docs/system/design-workflow.md)
- [State System](docs/system/state.md)
- [Global Rules](docs/system/rules.md)

## License

Turtle AI is licensed under the [MIT License](LICENSE). When copying or redistributing Turtle material, retain its copyright and license notice. The installation steps preserve a copy under `docs/licenses/` alongside your project's existing license.
