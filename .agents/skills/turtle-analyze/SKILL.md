---
name: turtle-analyze
description: Use this when analyzing a repository to build or refresh understanding of its architecture, modules, data flow, and testing patterns. Typically used when starting in a new repo, entering unfamiliar areas, or when repository context is incomplete or outdated. Do not use for feature planning, implementation, debugging, or routine small changes.
---

## Always read
- agents.md
- architecture.md
- repo_map.md

## External documentation rule
- If repository understanding depends on third-party framework, library, or SDK behavior, use Context7 to consult current official documentation before making assumptions.
- Prefer repository code as the source of truth for local behavior and architecture.
- If repo code and external docs conflict, call out the conflict explicitly and favor the repo for existing implementation intent.

## When to use
Run when:
- starting in a new repo
- repository understanding is incomplete
- entering an unfamiliar module
- repo structure has materially changed
- existing docs/analysis/repo_analysis.md is missing or stale

Do NOT run for every small feature.

---

## State awareness (CRITICAL)
- This workflow is plan-driven
- ANALYZE does NOT determine step execution
- Step state is NOT determined in this phase
- Step state is derived later using the Current Step Detector Pattern

---

## Inputs required
- repository code
- existing docs/analysis/repo_analysis.md (if present)
- relevant modules under review

---

## Output expected
- concise repository analysis in chat
- optionally updated docs/analysis/repo_analysis.md

---

## Purpose
- build or refresh repo understanding
- reduce repeated re-learning
- prevent incorrect assumptions during:
  - PLAN
  - EXECUTE
  - VERIFY
  - TEST

---

## Task
Read the repository and summarize:

- project architecture (high-level system design)
- key modules and their responsibilities
- data models and relationships
- API structure and boundaries
- important dependencies and integrations
- testing patterns (location, structure, frameworks)
- important conventions from agents.md
- potential risk areas (complex, fragile, or unclear parts of the system)

---

## Testing awareness (IMPORTANT)
Identify:
- how tests are structured in this repo
- where tests live
- naming conventions
- frameworks used (pytest, jest, etc.)

This ensures the TEST phase aligns with actual repository patterns and avoids incorrect assumptions.

---

## Output rules
- be concise and structured
- prefer facts over guesses
- clearly label assumptions and unknowns
- do NOT invent architecture, modules, or patterns

---

## Optional persistence
- create or update docs/analysis/repo_analysis.md only if analysis adds new or corrected understanding
- prefer updating existing content over duplicating sections

---

## Structure
## Repository Analysis
## Project Architecture
## Key Modules & Responsibilities
## Data Models
## API Structure
## Testing Patterns
## Dependencies & Integrations
## Risk Areas & Complexity
## Assumptions / Unknowns

---

## Rules
- this phase is for understanding only; do NOT propose changes
- do NOT modify code during analysis
- do NOT expand scope beyond understanding the current system
- treat code as source of truth over documentation
