# Planning

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start narrative -->
### EPIC-001 · approved · source: planning:EPIC-001

Restrict all chatbot access to signed-in staff via email/password authentication, with an admin role gating document management and session lifecycle (sign-out, expiry, deactivation) enforced as verifiable outcomes.

Goal: no public or anonymous access; admin role governs who can manage documents; session and sign-in security expressed as user/admin-verifiable outcomes. Stories: US-001 sign in with email/password; US-002 restrict access to authenticated users; US-003 admin role governs document management; US-004 end sessions on sign-out/expiry/deactivation.

**Related:** US-001, US-002, US-003, US-004

### EPIC-002 · approved · source: planning:EPIC-002

Admins build and maintain a single shared knowledge base by uploading, replacing and deleting documents in PDF, Word, plain text and Markdown; deletions remove content from retrieval so stale information is not used to answer questions.

Goal: single shared document collection visible to all staff, no per-document or per-user access restriction in scope (stated assumption). Stories: US-005 upload documents in supported formats; US-006 replace an existing document; US-007 delete a document and remove it from retrieval; US-008 view the shared knowledge base contents and indexing status.

**Related:** US-005, US-006, US-007, US-008

### EPIC-003 · approved · source: planning:EPIC-003

Uploaded documents are automatically chunked and converted to vector embeddings via Voyage AI, stored in a vector-capable datastore, so content can be retrieved at query time.

Goal: automatic chunking/embedding pipeline requiring no manual preparation. Assumptions surfaced: external embedding provider is Voyage AI requiring VOYAGE_API_KEY; embeddings held in a vector-capable datastore such as PostgreSQL with pgvector; hosting/region left to engineering with no stated data-residency requirement. Stories: US-009 automatically chunk and embed uploaded content; US-010 configure the embedding provider credential securely.

**Related:** US-009, US-010

### EPIC-004 · approved · source: planning:EPIC-004

Staff ask questions through a conversational chat interface; the system retrieves relevant passages from the knowledge base, sends them with the question to the Anthropic Claude API, and returns a grounded answer with citations to source documents and passages.

Goal: conversational RAG chat grounded in the shared knowledge base. Assumptions surfaced: answer generation uses the Anthropic Claude API requiring ANTHROPIC_API_KEY; answers must cite the source documents/passages they draw from. Stories: US-011 ask a question through the chat interface; US-012 retrieve relevant passages for a question; US-013 generate grounded answers with Claude; US-014 cite source documents and passages.

**Related:** US-011, US-012, US-013, US-014

### EPIC-005 · approved · source: planning:EPIC-005

Each staff member's conversations with the chatbot are saved and available to them across sessions, with history kept private to the individual user.

Goal: persistent, per-user, private conversation history. Assumption surfaced: conversation history is saved per user and not shared between users. Stories: US-015 save conversations per user; US-016 keep conversation history private to the user.

**Related:** US-015, US-016

### SPRINT-1 · approved · source: planning:Sprint 1

Sprint 1 builds core authentication: sign-in, session lifecycle (sign-out, expiry, deactivation), and access restriction foundations.

Goal: Core authentication: sign-in, session lifecycle (sign-out, expiry, deactivation), and access restriction foundations. Capacity: 20 points. Covers EPIC-001 and its stories US-001 (8 pts, backend-agent), US-002 (5 pts, backend-agent), US-003 (3 pts, backend-agent), US-004 (5 pts, backend-agent). Task breakdown (US-001): users table migration with secure password hash storage; sign-in and current-user endpoints; sign-in page and client-side session handling. Full item list is attached to the plan document word for word.

**Related:** EPIC-001, US-001, US-002, US-003, US-004

### SPRINT-2 · approved · source: planning:Sprint 2

Sprint 2 delivers admin-gated document management: role enforcement, embedding provider configuration, document upload, and knowledge base listing.

Goal: Admin-gated document management: role enforcement, embedding provider configuration, document upload, and knowledge base listing. Capacity: 20 points. Maps to EPIC-002 stories US-005 (8 pts, backend-agent) and US-008 (2 pts, frontend-agent), to EPIC-003 story US-010 (3 pts, integration-agent) for embedding-provider credential configuration, and references the role-enforcement outcomes of US-003 (EPIC-001). Task breakdown (US-005): documents table and status tracking; document upload endpoint with format validation and S3 storage; enqueue indexing job on upload; document upload UI with rejection messaging. Full item list is attached to the plan document word for word.

**Related:** EPIC-002, US-003, US-005, US-008, US-010

### SPRINT-3 · approved · source: planning:Sprint 3

Sprint 3 implements the automatic chunking/embedding pipeline, plus document replace and delete with correct removal from retrieval.

Goal: Automatic chunking/embedding pipeline, and document replace/delete with correct removal from retrieval. Capacity: 20 points. Covers EPIC-003 story US-009 (8 pts, backend-agent) and EPIC-002 stories US-006 (5 pts, backend-agent) and US-007 (3 pts, backend-agent). Task breakdown (US-009): text extraction and chunking in the indexing worker; Voyage AI embedding integration with error handling; store chunk embeddings in pgvector and mark document ready/failed; tests for successful and failed embedding paths. Full item list is attached to the plan document word for word.

**Related:** EPIC-003, US-006, US-007, US-009

### SPRINT-4 · approved · source: planning:Sprint 4

Sprint 4 builds passage retrieval and Claude-grounded answer generation with source citations.

Goal: Passage retrieval and Claude-grounded answer generation with source citations. Capacity: 20 points. Covers EPIC-004 stories US-012 (5 pts, backend-agent), US-013 (8 pts, integration-agent), and US-014 (5 pts, backend-agent). Task breakdown (US-013): Claude API call with retrieved passages as context; streaming response handling in the API; handle Claude failures with retryable error messaging; tests for grounded answer generation and failure paths. Full item list is attached to the plan document word for word.

**Related:** EPIC-004, US-012, US-013, US-014

### SPRINT-5 · approved · source: planning:Sprint 5

Sprint 5 delivers the end-to-end chat interface along with saved, private per-user conversation history.

Goal: End-to-end chat interface with saved, private per-user conversation history. Capacity: 20 points. Covers EPIC-004 story US-011 (8 pts, frontend-agent) and EPIC-005 stories US-015 (5 pts, backend-agent) and US-016 (3 pts, backend-agent). Task breakdown (US-011): chat conversation UI with message thread; responsive layout for smaller screens; wire chat UI to conversation and message endpoints with streaming display; tests for the chat interaction flow. Full item list is attached to the plan document word for word.

**Related:** EPIC-004, EPIC-005, US-011, US-015, US-016
<!-- sdlc:auto:end narrative -->
