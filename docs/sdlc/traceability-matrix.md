# Traceability Matrix

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start legend -->
Built from recorded links, not written by a model: requirement key → plan item (`source_stable_key`) → development ticket (`stable_key`) → the ticket's test steps → the last live deployment that started after the work finished. **AC tests** counts the story's acceptance criteria that have a test case, and those whose test cases all passed in the latest finished E2E run. **Tests** counts the tests written for the story, and **Result** how many last passed, how many failed and when, as of this page's generation; the Documentation tab shows them live.
<!-- sdlc:auto:end legend -->

<!-- sdlc:auto:start matrix -->
| Requirement | Epic | Title | Plan items | Sprint | Design | Code | Commits | Ticket | Test step | Tests | Result | AC tests | Deployed |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| US-001 | EPIC-001 | Sign in with email and password | US-001 | 1 | GET /auth/me, POST /auth/login, api, db, sessions, users | — | — | not started | not run | 0 | No tests | 0/3 tested · 0/3 passing | — |
| US-002 | EPIC-001 | Restrict access to authenticated users | US-002, US-002-T1, US-002-T2, US-002-T3 | 1 | GET /auth/me, api, sessions, web | — | — | not started | not run | 0 | No tests | 0/2 tested · 0/2 passing | — |
| US-003 | EPIC-001 | Admin role governs document management | US-003 | 1 | DELETE /documents/{id}, POST /documents, PUT /documents/{id}, api, documents, users, web | — | — | not started | not run | 0 | No tests | 0/2 tested · 0/2 passing | — |
| US-004 | EPIC-001 | End sessions on sign-out and expiry | US-004 | 2 | POST /auth/logout, api, db, sessions, users | — | — | not started | not run | 0 | No tests | 0/3 tested · 0/3 passing | — |
| US-005 | EPIC-002 | Upload documents in supported formats | US-005, US-005-T1, US-005-T2, US-005-T3 | 2 | GET /documents, POST /documents, api, documents, storage, web | — | — | not started | not run | 0 | No tests | 0/3 tested · 0/3 passing | — |
| US-006 | EPIC-002 | Replace an existing document | US-006 | 3 | PUT /documents/{id}, api, db, document_chunks, documents, storage, worker | — | — | not started | not run | 0 | No tests | 0/2 tested · 0/2 passing | — |
| US-007 | EPIC-002 | Delete a document and remove it from retrieval | US-007 | 4 | DELETE /documents/{id}, api, db, document_chunks, documents, storage | — | — | not started | not run | 0 | No tests | 0/2 tested · 0/2 passing | — |
| US-008 | EPIC-002 | View the shared knowledge base contents | US-008 | 2 | GET /documents, api, documents, web | — | — | not started | not run | 0 | No tests | 0/2 tested · 0/2 passing | — |
| US-009 | EPIC-003 | Automatically chunk and embed uploaded content | US-009, US-009-T1, US-009-T2, US-009-T3, US-009-T4 | 3 | db, document_chunks, documents, embeddings, queue, worker | — | — | not started | not run | 0 | No tests | 0/3 tested · 0/3 passing | — |
| US-010 | EPIC-003 | Configure the embedding provider credential | US-010 | 2 | embeddings, worker | — | — | not started | not run | 0 | No tests | 0/2 tested · 0/2 passing | — |
| US-011 | EPIC-004 | Ask a question through the chat interface | US-011, US-011-T1, US-011-T2, US-011-T3 | 4 | GET /conversations/{id}/messages, POST /conversations, POST /conversations/{id}/messages, api, conversations, messages, web | — | — | not started | not run | 0 | No tests | 0/2 tested · 0/2 passing | — |
| US-012 | EPIC-004 | Retrieve relevant passages for a question | US-012 | 5 | POST /conversations/{id}/messages, api, db, document_chunks, embeddings | — | — | not started | not run | 0 | No tests | 0/2 tested · 0/2 passing | — |
| US-013 | EPIC-004 | Generate grounded answers with Claude | US-013, US-013-T1, US-013-T2, US-013-T3 | 5 | POST /conversations/{id}/messages, ai_provider, api, messages, web | — | — | not started | not run | 0 | No tests | 0/3 tested · 0/3 passing | — |
| US-014 | EPIC-004 | Cite source documents and passages | US-014 | 5 | GET /conversations/{id}/messages, api, db, message_citations, web | — | — | not started | not run | 0 | No tests | 0/2 tested · 0/2 passing | — |
| US-015 | EPIC-005 | Save conversations per user | US-015 | 4 | GET /conversations, POST /conversations, api, conversations, db, messages | — | — | not started | not run | 0 | No tests | 0/2 tested · 0/2 passing | — |
| US-016 | EPIC-005 | Keep conversation history private to the user | US-016 | 4 | GET /conversations, api, conversations, db | — | — | not started | not run | 0 | No tests | 0/2 tested · 0/2 passing | — |
<!-- sdlc:auto:end matrix -->
