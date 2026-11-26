<!-- sdlc:auto:start adr -->
# ADR-0003: Use PostgreSQL 16 for Database

Status: Accepted  
Date: 2026-11-26

## Context and Problem Statement

Which Database technology does the project use? Decided in architecture v1.

## Considered Options

- PostgreSQL 16
- MySQL
- Dedicated vector database

## Decision Outcome

Chosen option: PostgreSQL 16, because Approved stack. One store holds users, sessions, document metadata, conversations, and — via the pgvector extension — the chunk embeddings, so retrieval needs no separate vector service at this modest volume.

## Consequences

pgvector similarity search is ample for ~50 users and a modest corpus; a dedicated vector DB would only matter at much larger scale, which is out of scope.
<!-- sdlc:auto:end adr -->