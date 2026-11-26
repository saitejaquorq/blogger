# Sprint Plan

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start sprints -->
## Sprint 1: Sprint 1

Core sign-in, authenticated-only access, and admin-role gating for document management.

Capacity: 20 points · Committed: 18

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-001 | Story | Sign in with email and password | 5 | backend-agent | — |
| US-002 | Story | Restrict access to authenticated users | 8 | backend-agent | US-001 |
| ↳ US-002-T1 | Task | Implement authentication dependency rejecting unauthenticated API requests | — | backend-agent | — |
| ↳ US-002-T2 | Task | Add route guards and sign-in redirects in the React app | — | frontend-agent | — |
| ↳ US-002-T3 | Task | Add tests confirming unauthenticated access is rejected on UI and API | — | qa-agent | — |
| US-003 | Story | Admin role governs document management | 5 | backend-agent | US-001, US-002 |

## Sprint 2: Sprint 2

Session expiry/deactivation, document upload, embedding-key configuration, and knowledge-base listing.

Capacity: 20 points · Committed: 19

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-004 | Story | End sessions on sign-out and expiry | 5 | backend-agent | US-001 |
| US-005 | Story | Upload documents in supported formats | 8 | backend-agent | US-003 |
| ↳ US-005-T1 | Task | Implement document upload endpoint | — | backend-agent | — |
| ↳ US-005-T2 | Task | Build admin upload UI with format validation and status list | — | frontend-agent | — |
| ↳ US-005-T3 | Task | Write tests covering accepted and rejected upload formats | — | qa-agent | — |
| US-008 | Story | View the shared knowledge base contents | 3 | frontend-agent | US-005 |
| US-010 | Story | Configure the embedding provider credential | 3 | devops-agent | — |

## Sprint 3: Sprint 3

Automatic chunking and embedding pipeline, and document replace.

Capacity: 20 points · Committed: 18

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-006 | Story | Replace an existing document | 5 | backend-agent | US-005, US-009 |
| US-009 | Story | Automatically chunk and embed uploaded content | 13 | integration-agent | US-005, US-010 |
| ↳ US-009-T1 | Task | Implement text extraction for PDF, Word, text and Markdown | — | backend-agent | — |
| ↳ US-009-T2 | Task | Implement chunking and Voyage AI embedding pipeline | — | integration-agent | — |
| ↳ US-009-T3 | Task | Wire Redis queue job processing with document status updates | — | devops-agent | — |
| ↳ US-009-T4 | Task | Handle missing/invalid embedding credential and provider errors | — | backend-agent | — |

## Sprint 4: Sprint 4

Document deletion, conversational chat interface, and per-user saved/private history.

Capacity: 20 points · Committed: 19

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-007 | Story | Delete a document and remove it from retrieval | 5 | backend-agent | US-005, US-009 |
| US-011 | Story | Ask a question through the chat interface | 8 | frontend-agent | US-001, US-002 |
| ↳ US-011-T1 | Task | Implement conversation and message API endpoints | — | backend-agent | — |
| ↳ US-011-T2 | Task | Build responsive chat UI | — | frontend-agent | — |
| ↳ US-011-T3 | Task | Write end-to-end test for asking a question and receiving a reply | — | qa-agent | — |
| US-015 | Story | Save conversations per user | 3 | backend-agent | US-011 |
| US-016 | Story | Keep conversation history private to the user | 3 | backend-agent | US-015, US-002 |

## Sprint 5: Sprint 5

Relevant-passage retrieval, Claude-grounded answers, and source citations.

Capacity: 20 points · Committed: 18

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-012 | Story | Retrieve relevant passages for a question | 5 | integration-agent | US-009 |
| US-013 | Story | Generate grounded answers with Claude | 8 | integration-agent | US-012 |
| ↳ US-013-T1 | Task | Implement Claude API integration for grounded answers | — | integration-agent | — |
| ↳ US-013-T2 | Task | Implement Claude failure handling | — | backend-agent | — |
| ↳ US-013-T3 | Task | Wire streaming answer delivery into the chat UI | — | frontend-agent | — |
| US-014 | Story | Cite source documents and passages | 5 | backend-agent | US-013 |

## Unscheduled

_Nothing unscheduled._
<!-- sdlc:auto:end sprints -->
