---
name: turtle-agents
description: Use this when creating or updating agents.md to define repository rules, coding conventions, and safety constraints. Typically used at project setup or when conventions are unclear or outdated. Do not use for planning, implementation, debugging, or testing.
---

## When to use
Use at repo setup time or whenever project rules, conventions, or safety constraints need to be created or refreshed.

## Inputs required
- project description
- repository structure
- team/repo conventions
- file naming rules
- security expectations

## Output expected
- `agents.md`
- concise repo rules
- naming conventions
- coding standards
- safety constraints
- architecture safety notes

## Rules
- Prefer updating an existing agents.md over replacing it entirely
- Keep rules concise and enforceable
- Align with existing repository structure and patterns
- Do not introduce new frameworks or tools unless already present
- Prioritize safety, clarity, and maintainability

### File Naming Conventions
- Use snake_case for all generated files
- Use underscores (_) instead of hyphens (-)
- Examples:
  - user_profile_page.md
  - payment_routing_plan.md
  - auth_service.py
- Never generate file names using hyphens (-)

# Repository Guidelines

## Project Overview
This is a  [project description]

## Project Structure & Module Organization

- Describe folders, major modules, and boundaries

## Build, Test, and Development Commands

- List core commands (install, run, test, lint)

## Coding Style & Naming Conventions

- Follow snake_case for files
- Follow existing language/framework conventions

## Testing Guidelines

- Define where tests live and naming patterns

## Commit & Pull Request Guidelines

- Use concise, descriptive commit messages

## Configuration & Environment

- Document required environment variables and secrets handling

## Security
- Never expose API keys.
- Validate all user input.

## Conventions

- Prefer simple, readable solutions
- Avoid unnecessary abstractions

## Architecture Safety

- Do not bypass established system boundaries
- Preserve data integrity and access patterns