---
name: turtle-mockup
description: Use this after a Turtle design audit to create or revise isolated, editable visual mockups and an implementation-oriented design specification. Do not use it to modify production code or approve a design for planning.
---

## Always read

- `agents.md`
- `architecture.md`
- `repo_map.md`
- `docs/system/design-workflow.md`
- `docs/design/<feature_slug>/audit.md`

Also read `docs/design/<feature_slug>/checkpoint.md` when it exists and its status is `changes_requested`.

If a foundation file is absent, inspect the repository directly and record the missing context in the design specification instead of inventing it.

## Preconditions

- `feature_slug` is known or can be derived from the design workspace.
- `docs/design/<feature_slug>/audit.md` exists.

If the audit is missing, stop and report:

```text
Mockup blocked. Run /turtle-design-audit for this feature first.
```

## Task

Create or revise only:

```text
docs/design/<feature_slug>/design-spec.md
docs/design/<feature_slug>/mockups/
```

The required visual artifact is responsive, editable HTML/CSS under `mockups/`. Keep it fully isolated from production code. Use realistic content and demonstrate the hierarchy, responsive behavior, and important states required by the audit.

Normally provide:

- one conservative direction emphasizing continuity and reuse
- one more ambitious direction when a genuinely different approach would help the user decide

One direction is sufficient when the user has already selected a clear direction or requested a narrow revision. Do not manufacture alternatives that differ only cosmetically.

## Visual output

- Prefer existing project colors, typography, spacing, assets, and recognizable component patterns when the audit says they should be preserved.
- Make each direction easy to identify and compare from `mockups/index.html`.
- Use semantic HTML and responsive CSS.
- Include visible focus, hover, empty, error, or loading states only when they matter to the proposed experience.
- Avoid production dependencies unless the mockup cannot communicate the design without them.
- Render preview images into `mockups/previews/` when a rendering capability is available; visually inspect the result and correct obvious layout problems.
- If preview rendering is unavailable, keep the HTML mockup usable and record that limitation in `design-spec.md`.

## Design specification

Document for each direction:

- intent and rationale
- page structure and visual hierarchy
- existing and proposed components
- tokens and assets
- responsive behavior
- interaction and content states
- accessibility considerations
- assumptions, tradeoffs, and implementation implications

Clearly distinguish existing design elements from proposed ones. Link to the corresponding mockup entry point and previews.

## Revision behavior

When `checkpoint.md` requests changes:

- address only the recorded feedback and necessary dependent adjustments
- preserve approved or positively reviewed decisions
- update the design specification to match the revised visual
- do not change checkpoint status or create approval artifacts

## Boundaries

- Do not modify production application code or assets.
- Do not import mockup files into the production application.
- Do not create or modify `approved-design.md`.
- Do not declare a direction approved.
- Do not modify backlog or plan state.
- Do not describe the mockup as production-ready code.

## Completion

Return links to the editable mockup and design specification, identify the available directions, summarize visual verification performed, and recommend running `/turtle-design-checkpoint` next.
