<!-- sdlc:auto:start adr -->
# ADR-0002: Use FastAPI 0.115 for Backend

Status: Accepted  
Date: 2026-11-24

## Context and Problem Statement

Which Backend technology does the project use? Decided in architecture v1.

## Considered Options

- FastAPI 0.115
- Flask
- Django REST Framework

## Decision Outcome

Chosen option: FastAPI 0.115, because The approved backend; its async streaming suits token-by-token Claude responses and Python has the strongest Anthropic and Voyage client support for orchestrating RAG.

## Consequences

Background chunking/embedding is offloaded to a separate RQ worker rather than blocking request handlers, adding one more process to run.
<!-- sdlc:auto:end adr -->