---
name: turtle-performance
description: Use this when reviewing a completed feature for performance risks across API, database, and frontend paths. Typically run after implementation and verification, before finalization. Do not use for planning, implementation, debugging, or testing.
---

## Always read
- agents.md
- architecture.md
- repo_map.md

## When to use
- after implementation is complete and verified
- when changes touch performance-sensitive paths (API, DB, rendering)

## Inputs
- feature_slug
- docs/plans/<feature_slug>_plan.md
- modified files (git diff)
- relevant API, DB, frontend, or rendering logic

## Output
- concise performance summary
- concrete risks with impact and locations
- practical fix recommendations
- no-action notes where optimization is unnecessary

## Before reviewing
- read docs/plans/<feature_slug>_plan.md
- review modified files (git diff)
- inspect API, DB, and frontend/rendering paths
- do not modify code; this step is read-only

## Rules
- avoid premature optimization; focus on real bottlenecks
- do NOT invent issues; base findings on code and patterns observed
- prefer simple, low-risk improvements
- treat code as source of truth over documentation
- be concise and structured

## Checks
Evaluate for:
- N+1 queries / inefficient DB access
- inefficient loops or algorithms (time/space)
- large payloads / over-fetching
- unnecessary re-renders or state churn
- repeated computation / lack of memoization
- duplicate or redundant API calls
- blocking operations on request/response path
- missing pagination / indexing opportunities
- unbounded concurrency or missing timeouts

## Output Format
Performance Summary
- [concise]

Risks
- [issue]
  - Impact: low | medium | high
  - File: path
  - Why: short
  - Fix: short

No-action Notes
- [bullets]

Recommendations
- [bullets]

## Constraints
- do NOT modify any files
- do NOT include unrelated commentary
- keep output minimal and actionable
- if context is incomplete, state assumptions briefly in No-action Notes