# Sprint Plan

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start sprints -->
## Sprint 1: Sprint 1

Core authentication: sign-in, session lifecycle (sign-out, expiry, deactivation), and access restriction foundations.

Capacity: 20 points · Committed: 18

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-001 | Story | Sign in with email and password | 8 | backend-agent | — |
| ↳ US-001-T1 | Task | Add users table migration with secure password hash storage | — | backend-agent | — |
| ↳ US-001-T2 | Task | Implement sign-in and current-user endpoints | — | backend-agent | — |
| ↳ US-001-T3 | Task | Build sign-in page and client-side session handling | — | frontend-agent | — |
| US-002 | Story | Restrict access to authenticated users | 5 | backend-agent | US-001 |
| US-004 | Story | End sessions on sign-out and expiry | 5 | backend-agent | US-001 |

## Sprint 2: Sprint 2

Admin-gated document management: role enforcement, embedding provider configuration, document upload, and knowledge base listing.

Capacity: 20 points · Committed: 16

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-003 | Story | Admin role governs document management | 3 | backend-agent | US-002 |
| US-005 | Story | Upload documents in supported formats | 8 | backend-agent | US-003 |
| ↳ US-005-T1 | Task | Add documents table and status tracking | — | backend-agent | — |
| ↳ US-005-T2 | Task | Implement document upload endpoint with format validation and S3 storage | — | backend-agent | — |
| ↳ US-005-T3 | Task | Enqueue indexing job on upload | — | backend-agent | — |
| ↳ US-005-T4 | Task | Build document upload UI with rejection messaging | — | frontend-agent | — |
| US-008 | Story | View the shared knowledge base contents | 2 | frontend-agent | US-005 |
| US-010 | Story | Configure the embedding provider credential | 3 | integration-agent | — |

## Sprint 3: Sprint 3

Automatic chunking/embedding pipeline, and document replace/delete with correct removal from retrieval.

Capacity: 20 points · Committed: 16

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-006 | Story | Replace an existing document | 5 | backend-agent | US-005, US-009 |
| US-007 | Story | Delete a document and remove it from retrieval | 3 | backend-agent | US-005, US-009 |
| US-009 | Story | Automatically chunk and embed uploaded content | 8 | backend-agent | US-005, US-010 |
| ↳ US-009-T1 | Task | Implement text extraction and chunking in the indexing worker | — | backend-agent | — |
| ↳ US-009-T2 | Task | Integrate Voyage AI embedding calls with error handling | — | integration-agent | — |
| ↳ US-009-T3 | Task | Store chunk embeddings in pgvector and mark document ready/failed | — | backend-agent | — |
| ↳ US-009-T4 | Task | Add tests for successful and failed embedding paths | — | qa-agent | — |

## Sprint 4: Sprint 4

Passage retrieval and Claude-grounded answer generation with source citations.

Capacity: 20 points · Committed: 18

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-012 | Story | Retrieve relevant passages for a question | 5 | backend-agent | US-009 |
| US-013 | Story | Generate grounded answers with Claude | 8 | integration-agent | US-012 |
| ↳ US-013-T1 | Task | Implement Claude API call with retrieved passages as context | — | integration-agent | — |
| ↳ US-013-T2 | Task | Add streaming response handling in the API | — | backend-agent | — |
| ↳ US-013-T3 | Task | Handle Claude failures with retryable error messaging | — | backend-agent | — |
| ↳ US-013-T4 | Task | Add tests for grounded answer generation and failure paths | — | qa-agent | — |
| US-014 | Story | Cite source documents and passages | 5 | backend-agent | US-013 |

## Sprint 5: Sprint 5

End-to-end chat interface with saved, private per-user conversation history.

Capacity: 20 points · Committed: 16

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-011 | Story | Ask a question through the chat interface | 8 | frontend-agent | US-013, US-002 |
| ↳ US-011-T1 | Task | Build chat conversation UI with message thread | — | frontend-agent | — |
| ↳ US-011-T2 | Task | Implement responsive layout for smaller screens | — | frontend-agent | — |
| ↳ US-011-T3 | Task | Wire chat UI to conversation and message endpoints with streaming display | — | frontend-agent | — |
| ↳ US-011-T4 | Task | Add tests for the chat interaction flow | — | qa-agent | — |
| US-015 | Story | Save conversations per user | 5 | backend-agent | US-011 |
| US-016 | Story | Keep conversation history private to the user | 3 | backend-agent | US-015, US-002 |

## Unscheduled

_Nothing unscheduled._
<!-- sdlc:auto:end sprints -->
