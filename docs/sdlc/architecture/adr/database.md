<!-- sdlc:auto:start adr -->
# ADR-0003: Use PostgreSQL 16 for Database

Status: Accepted  
Date: 2026-11-26

## Context and Problem Statement

Which Database technology does the project use? Decided in architecture v2.

## Considered Options

- PostgreSQL 16
- MySQL
- Dedicated vector database

## Decision Outcome

Chosen option: PostgreSQL 16, because One store holds users, roles, sessions, conversations, document metadata and, via the pgvector extension, the embeddings and similarity search — no second database for this modest document volume.

## Consequences

pgvector at very large scale would need tuning, but ~50 users and modest documents sit well within a single small instance.
<!-- sdlc:auto:end adr -->