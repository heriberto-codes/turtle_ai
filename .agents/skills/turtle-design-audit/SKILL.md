---
name: turtle-design-audit
description: Use this to inspect an existing user interface, frontend implementation, or design system and create an evidence-based design audit before mockup work. Do not use it to generate mockups, approve a direction, or modify production code.
---

## Always read

- `agents.md`
- `architecture.md`
- `repo_map.md`
- `docs/system/design-workflow.md`

If a foundation file is absent, inspect the repository directly and record the missing context in the audit instead of inventing it.

## Inputs

- `feature_slug`
- user or business objective
- intended audience and primary action, when known
- current screenshots, a locally accessible interface, or frontend files
- relevant components, styles, design tokens, assets, and brand constraints

Normalize `feature_slug` to lowercase snake_case using only letters, numbers, and underscores.

## Task

Inspect the current experience and create:

```text
docs/design/<feature_slug>/audit.md
```

Ground findings in observable interface behavior, screenshots, repository code, or supplied requirements. Distinguish usability and accessibility problems from subjective visual recommendations.

When screenshots or a runnable interface are available, inspect them visually. When they are unavailable, audit the relevant code and state that visual verification remains incomplete.

## Audit content

Include concise sections for:

- Objective and audience
- Evidence inspected
- Current design inventory
- Strengths to preserve
- Findings, ordered by impact
- Existing components, tokens, and assets to reuse
- Responsive and accessibility considerations
- Constraints, assumptions, and missing evidence
- Prioritized opportunities for `turtle-mockup`

For each significant finding, explain the observed evidence, likely user impact, and recommended design response without prescribing implementation code.

## Existing artifact behavior

If `audit.md` already exists, read it first and update it incrementally. Preserve still-valid evidence and decisions, remove findings disproven by current evidence, and avoid duplicating observations.

## Boundaries

- Only create or update `docs/design/<feature_slug>/audit.md`.
- Do not create mockups or `design-spec.md`.
- Do not create or modify checkpoint or approval artifacts.
- Do not modify production code, production assets, backlog state, or plan state.
- Do not present personal taste as a requirement.
- Do not claim visual inspection occurred when only source code was inspected.

## Completion

Return the audit path, the highest-impact findings, reusable design elements discovered, and any evidence needed before mockup work can be reliable.
