/ideate_prompt

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

Based on the repository architecture, current codebase patterns, and any obvious product gaps, suggest useful improvements or missing features.

Prioritize:
- small, high-leverage features
- architecture improvements that reduce risk or complexity
- UX improvements grounded in existing product behavior
- missing tests, validation, or developer experience improvements

Avoid:
- speculative ideas not supported by the repo
- large rewrites
- new frameworks
- features that do not fit the current system

Output format

Features
- [concise idea]

Architecture Improvements
- [concise idea]

Client Improvements
- [concise idea]

Rules
- be practical
- prefer ideas that are realistically implementable in this repo
- be concise