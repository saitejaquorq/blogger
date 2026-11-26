<!-- sdlc:auto:start adr -->
# ADR-0002: Use FastAPI 0.115 for Backend

Status: Accepted  
Date: 2026-11-26

## Context and Problem Statement

Which Backend technology does the project use? Decided in architecture v2.

## Considered Options

- FastAPI 0.115
- Flask
- Django REST Framework

## Decision Outcome

Chosen option: FastAPI 0.115, because Python backend orchestrates RAG and streams Claude responses with server-sent events; the strongest ecosystem for the Anthropic and Voyage SDKs. Also runs the arq worker process for async indexing.

## Consequences

Async code and a separate worker process add a little operational surface, but keep uploads non-blocking for the admin.
<!-- sdlc:auto:end adr -->