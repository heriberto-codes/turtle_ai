/architecture_prompt

## When to use
Use when creating or refreshing the system blueprint for the repository.

## Inputs required
- current repository structure
- existing `architecture.md` if present
- major app components
- frameworks and services detected in repo
- known integrations and constraints

## Output expected
- `architecture.md`
- concise system blueprint
- project structure
- runtime components
- request/data flow
- architecture principles
- risks, extension points, and open questions

Create or maintain the system architecture blueprint.

This is a living document designed to give agents and engineers a fast, accurate understanding of the codebase so they can safely navigate and modify it.

Before writing:
- check if architecture.md exists in the project root
- if it exists, read it and extend/update it
- if it does not exist, create it

Rules
- do NOT overwrite existing valid content
- do NOT duplicate sections
- keep content concise and structured
- align with actual repository structure and patterns
- prefer repository facts over guesses
- if something is unclear, state it as an assumption or open question
- only document components that actually exist in the repo

Save path:
architecture.md

## Overview
- What this system does
- Primary users
- Main business purpose

## Project Structure
- High-level directory layout
- Major folders and responsibilities
- Important entry points

## System Boundaries
- What is inside the system
- What is outside the system
- External services and APIs
- Third-party integrations

## High-Level System Diagram
- Simple text diagram of major components and interactions

## Architectural Principles
- Core engineering principles
- Patterns to prefer / avoid

## Constraints and Non-Goals

## Runtime Components

## Request Flow

## Data Flow

## Data Layer

## Database Schema (High Level)

## Client Architecture

## Server Architecture

## API Structure

## Authentication

## Authorization

## Security Model

## Background Tasks

## Performance Considerations

## Observability and Operations

## Build and Run

## Environment Configuration

## Deployment

## Key Dependencies

## Known Risks

## Extension Points

## Open Questions

## Glossary / Acronyms