<!-- sdlc:auto:start adr -->
# ADR-0003: Use PostgreSQL 16 for Database

Status: Accepted  
Date: 2026-11-24

## Context and Problem Statement

Which Database technology does the project use? Decided in architecture v1.

## Considered Options

- PostgreSQL 16
- MySQL
- A dedicated vector database

## Decision Outcome

Chosen option: PostgreSQL 16, because The approved database; a single store holds users, sessions, document metadata, conversation history and — with the pgvector extension — the chunk embeddings, so retrieval runs without a second service at this modest volume.

## Consequences

pgvector at very large scale would need more tuning, but for ~50 staff and modest document volume an ivfflat/hnsw index is ample.
<!-- sdlc:auto:end adr -->