# Engineering workflow skill naming plan

Feature slug: `engineer_skill_naming`

Backlog item: standardize Engineering workflow skill names

## Architecture and scope

Turtle uses repo-scoped Markdown skills. The skill directory and YAML `name` identify the command. The rename introduces no runtime, API, data, or frontend changes.

Apply `turtle-engineer-` to the existing `execute`, `verify`, `test`, `debug`, `plan-step-update`, `security`, `performance`, `backlog-update`, `document`, `add`, `commit`, `push`, and `pull-request` skills. Keep `turtle-engineer-checkpoint` and all earlier workflow phases under their current names. Preserve all 25 skills, behavior, arguments, gate order, and state ownership. Use a direct rename without aliases.

## Risks and validation

Stale commands can break skill discovery, handoffs, or the completion gate. Validate all skill names against their directories, resolve command references and links, review the diff for behavior or state changes, and check discovery in a fresh Codex chat without invoking Git actions. Existing installations should preserve local customizations when replacing legacy folders.

## Step-by-step plan

- [x] Rename the 13 skill directories and their frontmatter in `.agents/skills/`, update all affected handoffs and references in the skills, `AGENTS.md`, `architecture.md`, `repo_map.md`, `docs/system/`, `docs/guides/planning_workflow.md`, the two existing feature records, and `README.md`, and add README upgrade guidance while preserving completed plans.
