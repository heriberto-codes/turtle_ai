---
name: turtle-ideate
description: Use this when generating grounded feature ideas and improvements based on the current repository. Typically used at the start of the discovery phase or when identifying gaps in UX, architecture, testing, or developer experience. Do not use for planning, implementation, debugging, or backlog structuring.
---

## Always read
- agents.md
- architecture.md
- repo_map.md

## When to use
Use when you want grounded feature ideas or improvements based on the current repository.

## Inputs required
- repository structure
- architecture.md
- current product behavior
- obvious gaps in UX, architecture, tests, or DX

## Output expected
- concise idea list
- grouped into:
  - Features
  - Architecture Improvements
  - Client Improvements

## Before writing
- read repository structure and key modules
- if an existing ideation document exists, preserve its structure while extending it incrementally
- prefer refining or merging ideas over creating duplicates

## Rules
- this phase is for ideation only; do NOT plan implementation details
- do NOT invent features that are not supported by the current system
- avoid duplication; merge with similar existing ideas
- keep ideas concise, practical, and scoped to the current repo
- prefer small, high-leverage improvements over large rewrites
- treat code as source of truth over assumptions

Based on the repository architecture, existing code patterns, and observable product behavior, suggest grounded improvements or missing features.

## Prioritization
Prioritize:
- small, high-leverage features
- architecture improvements that reduce risk or complexity
- UX improvements grounded in existing product behavior
- missing tests, validation, or developer experience improvements

## Avoid
Avoid:
- speculative ideas not supported by the repo
- large rewrites
- new frameworks
- features that do not fit the current system

## Output

### Features
- [concise idea]

### Architecture Improvements
- [concise idea]

### Client Improvements
- [concise idea]

## Notes
- prioritize highest-impact ideas first within each section
- call out assumptions explicitly if repository context is incomplete
- ensure all ideas are traceable to observed gaps or patterns in the codebase
