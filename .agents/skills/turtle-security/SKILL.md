---
name: turtle-security
description: Use this when reviewing a completed feature for security risks across authentication, authorization, data handling, and API boundaries. Typically run after implementation and verification, before finalization. Do not use for planning, implementation, debugging, or testing.
---

## Always read
- agents.md
- architecture.md
- repo_map.md

## When to use
- after implementation is complete and verified
- when changes touch auth, data handling, APIs, or input validation

## Inputs
- feature_slug
- docs/plans/<feature_slug>_plan.md
- modified files (git diff)
- relevant auth/validation/config code

## Output
- concise security summary
- concrete risks with severity and locations
- practical fix recommendations
- missing safeguards
- safe patterns observed

## Before reviewing
- read docs/plans/<feature_slug>_plan.md
- review modified files (git diff)
- inspect auth, validation, secrets, and API boundaries
- do not modify code; this step is read-only

## Rules
- focus on real risks; avoid speculation
- prioritize authentication, authorization, and data safety
- do NOT invent issues; base findings on code and patterns observed
- prefer simple, low-risk fixes
- treat code as source of truth over documentation
- be concise and structured

## Checks
Evaluate for:
- missing or incorrect authentication
- missing or incorrect authorization (RBAC/permissions)
- unsafe input handling (validation/sanitization)
- injection risks (SQL/NoSQL/command)
- XSS / CSRF risks
- secret exposure (keys, tokens, env)
- insecure logging (PII/secrets)
- unsafe file handling (uploads, paths)
- overly permissive APIs (CORS, methods)
- missing rate limiting / abuse protection

## Output Format
Security Summary
- [concise]

Risks
- [issue]
  - Severity: low | medium | high
  - File: path
  - Why: short
  - Fix: short

Missing Safeguards
- [bullets]

Safe Patterns
- [bullets]

## Constraints
- do NOT modify any files
- do NOT include unrelated commentary
- keep output minimal and actionable
- if context is incomplete, state assumptions briefly in Missing Safeguards