---
name: turtle-agents
description: Use this when creating, migrating, or updating AGENTS.md to define repository rules, coding conventions, and safety constraints. Typically used at project setup or when conventions are unclear or outdated. Do not use for planning, implementation, debugging, or testing.
---

## When to use

Use at repository setup time or whenever project rules, conventions, or safety constraints need to be created or refreshed.

## Inspect first

- repository structure, configuration, and existing conventions
- `architecture.md` when it exists
- `repo_map.md` when it exists
- root `AGENTS.md` and legacy root `agents.md`

This skill may run first in a new project. Missing `architecture.md` or `repo_map.md` must not block it. Inspect the repository directly, create a factual baseline, and leave unknowns explicit. Later runs may refine `AGENTS.md` after the other foundation documents exist.

## Instruction-file resolution

Apply these cases deterministically:

1. If only `AGENTS.md` exists, update it incrementally and preserve valid content.
2. If only legacy `agents.md` exists, preserve its valid content while migrating it to `AGENTS.md`, then remove `agents.md`.
3. If neither file exists, create `AGENTS.md`.
4. If both files exist, stop without modifying either file and ask the user to reconcile them. Never merge or delete conflicting instruction files silently.

Determine these cases from exact root directory entry names and, for tracked files, the Git index. Do not use case-insensitive path existence checks, which can make one file appear to have both names. On a case-insensitive filesystem, perform a legacy case-only migration through a unique temporary filename so Git and the filesystem record the uppercase canonical name.

`AGENTS.md` is the canonical filename. Create no new lowercase `agents.md` file. The root `AGENTS.md` in the Turtle AI repository is guidance for maintaining Turtle itself, not a template to copy verbatim into another project.

## Output expected

- canonical root `AGENTS.md`
- concise, enforceable repository rules
- project structure and naming conventions
- verified build, test, and development commands when available
- security and architecture safety constraints
- no remaining legacy `agents.md` after a successful one-file migration

## Content guidance

Keep `AGENTS.md` concise because it is loaded as repository context. Include only rules that should govern most work in the project:

- project purpose and actual structure
- build, test, lint, and development commands supported by the repository
- coding and file-naming conventions
- testing expectations
- commit and pull-request expectations when established
- configuration and secrets handling
- protected boundaries, generated files, and destructive-operation constraints

Put task-specific procedures in skills or dedicated documentation rather than duplicating them in `AGENTS.md`.

## Rules

- Base every rule on repository evidence or explicit user direction.
- Prefer updating valid content over replacing it wholesale.
- Treat code and configuration as the source of truth when documentation conflicts with them.
- Do not invent files, modules, commands, frameworks, services, or team conventions.
- Do not introduce tools or frameworks that the repository does not use.
- State uncertain details as unknown instead of guessing.
- Never expose API keys, credentials, tokens, personal data, or `.env` contents.
- Preserve established system boundaries, data integrity, and access patterns.
- Prefer simple, readable, maintainable guidance.

## File naming

- Follow existing language and framework conventions.
- Use snake_case for Turtle-generated project artifacts unless an external contract requires another name.
- `AGENTS.md` is an explicit uppercase filename required by the Codex repository-instructions contract.
- Do not rename existing project files merely to enforce a general naming preference.

## Foundation sequence

For a new project, use:

```text
turtle-agents -> turtle-architecture -> turtle-repo-map
```

`turtle-architecture` may run before `repo_map.md` exists. `turtle-repo-map` runs after both `AGENTS.md` and `architecture.md` are available.
