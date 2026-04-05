/analyze_prompt

## When to use
Run when:
- starting in a new repo
- repository understanding is incomplete
- entering an unfamiliar module
- repo structure has materially changed
- existing docs/analysis/repo_analysis.md is missing or stale

Do NOT run for every small feature.

## Inputs required
- repository code
- existing `docs/analysis/repo_analysis.md` if present
- architecture.md
- repo_map.md
- relevant modules under review

## Output expected
- concise repository analysis in chat
- optionally updated docs/analysis/repo_analysis.md

Purpose:
- build or refresh repo understanding
- reduce repeated re-learning
- lower bad assumptions

Task
Read the repository and summarize:

- project architecture
- key modules
- data models
- API structure
- important dependencies
- potential risk areas

List assumptions made due to missing information.

Output rules
- be concise
- prefer facts over guesses
- clearly state assumptions

Optional persistence
- create or update docs/analysis/repo_analysis.md only if needed

Structure:

# Repository Analysis

## Project Architecture
## Key Modules
## Data Models
## API Structure
## Important Dependencies
## Risk Areas
## Assumptions / Unknowns