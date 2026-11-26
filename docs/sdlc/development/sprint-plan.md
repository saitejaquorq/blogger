# Sprint Plan

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start sprints -->
## Sprint 1: Sprint 1

Stand up sign-in, sessions, admin RBAC and the Voyage embedding credential so every later feature has a secure, configured base to build on.

Capacity: 20 points · Committed: 19

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-001 | Story | Sign in with email and password | 8 | backend-agent | — |
| ↳ US-001-T1 | Task | Add users/sessions schema and secure password hashing | — | backend-agent | — |
| ↳ US-001-T2 | Task | Implement POST /auth/login and GET /auth/me | — | backend-agent | — |
| ↳ US-001-T3 | Task | Build sign-in page | — | frontend-agent | — |
| US-002 | Story | Restrict access to authenticated users | 5 | backend-agent | US-001 |
| US-003 | Story | Admin role governs document management | 3 | backend-agent | US-001, US-002 |
| US-010 | Story | Configure the embedding provider credential | 3 | devops-agent | — |

## Sprint 2: Sprint 2

Deliver document upload/storage with listing, and the conversational chat UI with its message endpoints.

Capacity: 20 points · Committed: 19

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-005 | Story | Upload documents in supported formats | 8 | backend-agent | US-003 |
| ↳ US-005-T1 | Task | Implement POST /documents with S3 upload and metadata record | — | backend-agent | — |
| ↳ US-005-T2 | Task | Add format validation and rejection messaging | — | backend-agent | — |
| ↳ US-005-T3 | Task | Build admin upload and document list UI | — | frontend-agent | — |
| US-008 | Story | View the shared knowledge base contents | 3 | frontend-agent | US-005 |
| US-011 | Story | Ask a question through the chat interface | 8 | frontend-agent | US-002 |
| ↳ US-011-T1 | Task | Implement conversation and message creation endpoints | — | backend-agent | — |
| ↳ US-011-T2 | Task | Build responsive chat interface | — | frontend-agent | — |
| ↳ US-011-T3 | Task | Wire chat UI to messaging endpoints | — | frontend-agent | — |

## Sprint 3: Sprint 3

Build the automatic chunking-and-embedding pipeline and complete session expiry/deactivation behaviour.

Capacity: 20 points · Committed: 18

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-004 | Story | End sessions on sign-out and expiry | 5 | backend-agent | US-001 |
| US-009 | Story | Automatically chunk and embed uploaded content | 13 | backend-agent | US-005, US-010 |
| ↳ US-009-T1 | Task | Trigger indexing job on upload via Redis queue | — | devops-agent | — |
| ↳ US-009-T2 | Task | Implement text extraction and chunking | — | backend-agent | — |
| ↳ US-009-T3 | Task | Call Voyage AI and store embeddings in pgvector | — | integration-agent | — |
| ↳ US-009-T4 | Task | Handle status transitions and embedding failures | — | backend-agent | — |

## Sprint 4: Sprint 4

Implement passage retrieval and Claude-grounded answer generation, and remove deleted documents from retrieval.

Capacity: 20 points · Committed: 19

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-007 | Story | Delete a document and remove it from retrieval | 3 | backend-agent | US-005, US-009 |
| US-012 | Story | Retrieve relevant passages for a question | 8 | integration-agent | US-009, US-011 |
| ↳ US-012-T1 | Task | Embed incoming question via Voyage AI | — | integration-agent | — |
| ↳ US-012-T2 | Task | Implement pgvector similarity search | — | backend-agent | — |
| ↳ US-012-T3 | Task | Handle no-sufficiently-relevant-match case | — | backend-agent | — |
| US-013 | Story | Generate grounded answers with Claude | 8 | integration-agent | US-012 |
| ↳ US-013-T1 | Task | Integrate Anthropic Claude API with retrieved context | — | integration-agent | — |
| ↳ US-013-T2 | Task | Stream answer to the chat UI | — | backend-agent | — |
| ↳ US-013-T3 | Task | Handle Claude API failures | — | backend-agent | — |

## Sprint 5: Sprint 5

Add source citations, per-user conversation history with privacy enforcement, and document replace.

Capacity: 20 points · Committed: 18

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-006 | Story | Replace an existing document | 5 | backend-agent | US-005, US-009 |
| US-014 | Story | Cite source documents and passages | 5 | backend-agent | US-013 |
| US-015 | Story | Save conversations per user | 5 | backend-agent | US-011 |
| US-016 | Story | Keep conversation history private to the user | 3 | backend-agent | US-015, US-002 |

## Unscheduled

_Nothing unscheduled._
<!-- sdlc:auto:end sprints -->
