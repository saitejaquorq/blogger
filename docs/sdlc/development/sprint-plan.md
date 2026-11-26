# Sprint Plan

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start sprints -->
## Sprint 1: Sprint 1

Stand up authentication, session lifecycle and admin-role gating so every later feature can sit behind sign-in

Capacity: 20 points · Committed: 18

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-001 | Story | Sign in with email and password | 5 | backend-agent | — |
| US-002 | Story | Restrict access to authenticated users | 5 | backend-agent | US-001 |
| US-003 | Story | Admin role governs document management | 3 | backend-agent | US-001, US-002 |
| US-004 | Story | End sessions on sign-out and expiry | 5 | backend-agent | US-001 |

## Sprint 2: Sprint 2

Enable document upload, secure embedding-provider configuration, and visibility into the knowledge base

Capacity: 20 points · Committed: 14

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-005 | Story | Upload documents in supported formats | 8 | backend-agent | US-003 |
| ↳ US-005-T1 | Task | Implement document upload endpoint with format validation and S3 storage | — | backend-agent | — |
| ↳ US-005-T2 | Task | Build admin upload UI with format validation feedback | — | frontend-agent | — |
| ↳ US-005-T3 | Task | Add document status tracking fields and listing query | — | backend-agent | — |
| US-008 | Story | View the shared knowledge base contents | 3 | frontend-agent | US-005 |
| US-010 | Story | Configure the embedding provider credential | 3 | devops-agent | — |

## Sprint 3: Sprint 3

Automatically chunk and embed documents, and support replacing/deleting content safely

Capacity: 20 points · Committed: 18

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-006 | Story | Replace an existing document | 5 | backend-agent | US-005, US-009 |
| US-007 | Story | Delete a document and remove it from retrieval | 5 | backend-agent | US-005, US-009 |
| US-009 | Story | Automatically chunk and embed uploaded content | 8 | backend-agent | US-005, US-010 |
| ↳ US-009-T1 | Task | Implement text extraction and chunking pipeline for PDF/Word/txt/md | — | backend-agent | — |
| ↳ US-009-T2 | Task | Integrate Voyage AI embeddings API call per chunk | — | integration-agent | — |
| ↳ US-009-T3 | Task | Store chunk embeddings in pgvector and update document status | — | backend-agent | — |

## Sprint 4: Sprint 4

Deliver the end-to-end RAG chat experience: retrieval, Claude-grounded answers and citations

Capacity: 20 points · Committed: 18

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-011 | Story | Ask a question through the chat interface | 5 | frontend-agent | US-002, US-013 |
| US-012 | Story | Retrieve relevant passages for a question | 5 | backend-agent | US-009 |
| US-013 | Story | Generate grounded answers with Claude | 5 | integration-agent | US-012 |
| US-014 | Story | Cite source documents and passages | 3 | frontend-agent | US-013 |

## Sprint 5: Sprint 5

Persist and protect each user's conversation history

Capacity: 20 points · Committed: 8

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-015 | Story | Save conversations per user | 5 | backend-agent | US-011 |
| US-016 | Story | Keep conversation history private to the user | 3 | backend-agent | US-015 |

## Unscheduled

_Nothing unscheduled._
<!-- sdlc:auto:end sprints -->
