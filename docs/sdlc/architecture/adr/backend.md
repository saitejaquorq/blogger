<!-- sdlc:auto:start adr -->
# ADR-0002: Use FastAPI 0.115 for Backend

Status: Accepted  
Date: 2026-11-24

## Context and Problem Statement

Which Backend technology does the project use? Decided in architecture v1.

## Considered Options

- FastAPI 0.115
- Django
- Flask

## Decision Outcome

Chosen option: FastAPI 0.115, because Python hosts the Anthropic and Voyage SDKs and FastAPI's streaming responses suit the chat experience; it orchestrates retrieval, Claude calls, auth and enqueuing indexing jobs.

## Consequences

Async orchestration plus a separate RQ worker is more moving parts than a single sync app, but keeps uploads from blocking admins.
<!-- sdlc:auto:end adr -->