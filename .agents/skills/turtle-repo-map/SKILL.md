---
name: turtle-repo-map
description: Use this when creating or updating repo_map.md to provide a high-level navigation map of the repository, including key directories, modules, entry points, and protected paths. Typically used when onboarding, analyzing a new codebase, or when repository structure has changed. Do not use for planning, implementation, debugging, or testing.
---

## When to use
Use when creating or refreshing a high-level navigation map of the repository.

## Inputs required
- repository directory structure
- major folders and modules
- protected paths
- important entry points
- documentation locations

## Always read
- agents.md
- architecture.md

## Output expected
- `repo_map.md`
- top-level repo navigation
- key module locations
- protected paths
- important file/folder references

## Before writing
- check if repo_map.md exists in the project root
- if it exists, read it and preserve its structure while extending it incrementally
- prefer editing existing sections over creating new ones; avoid duplication
- if it does not exist, create it

## Rules
- base the map on the actual repository structure
- do NOT invent folders, modules, or files that do not exist
- prioritize clarity and high-level navigation over detail
- keep descriptions concise and scannable
- if documentation conflicts with code, treat code as source of truth

## Repository Navigation Map

## Top-Level
- list relevant directories and key files

## Client 
- list relevant directories and key files

## Server (API)
- list relevant directories and key files

## Data
- list relevant directories and key files

## Build Output
- list relevant directories and key files

## Documentation
- list relevant directories and key files

## Protected Paths (Do Not Modify)
- node_modules/
- build/
- dist/
- migrations/ (unless explicitly requested)
- .env files

## Notes
- highlight important entry points (e.g., main server file, app entry, config)
- call out any unusual or non-standard structure if present