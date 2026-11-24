<!-- sdlc:auto:start adr -->
# ADR-0003: Use PostgreSQL 16 for Database

Status: Accepted  
Date: 2026-11-24

## Context and Problem Statement

Which Database technology does the project use? Decided in architecture v1.

## Considered Options

- PostgreSQL 16
- MySQL
- MongoDB

## Decision Outcome

Chosen option: PostgreSQL 16, because One database holds users, sessions, conversations, document metadata and, via the pgvector extension, the embeddings that power nearest-neighbour retrieval, avoiding a second vector service for this modest volume.

## Consequences

pgvector on a single RDS instance scales fine for ~50 users and a modest corpus but would need tuning (or a dedicated vector store) at far larger scale.
<!-- sdlc:auto:end adr -->