<!-- sdlc:auto:start adr -->
# ADR-0002: Use FastAPI 0.115 for Backend

Status: Accepted  
Date: 2026-11-26

## Context and Problem Statement

Which Backend technology does the project use? Decided in architecture v1.

## Considered Options

- FastAPI 0.115
- Django
- Flask

## Decision Outcome

Chosen option: FastAPI 0.115, because Approved stack. Python gives first-class Anthropic and Voyage libraries, and FastAPI's streaming responses suit a chat experience. It orchestrates embed-retrieve-generate and enqueues indexing work.

## Consequences

Async orchestration and background work must be structured carefully; long-running indexing is offloaded to a separate worker rather than blocking request handlers.
<!-- sdlc:auto:end adr -->