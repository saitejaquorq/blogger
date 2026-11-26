# Planning

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start narrative -->
### EPIC-001 · approved · source: planning:EPIC-001

Staff Authentication and Role-Based Access.

All access to the chatbot is restricted to signed-in staff via email/password accounts; no public or anonymous access. An admin role governs who can manage documents; session and sign-in security are expressed as verifiable outcomes.

**Related:** US-001, US-002, US-003, US-004

### EPIC-002 · approved · source: planning:EPIC-002

Document Management and Knowledge Base.

Admins build and maintain a single shared knowledge base by uploading PDF, Word (.docx), plain text and Markdown documents; they can add, replace and delete documents, and deletion removes content from retrieval so stale information is no longer used. Assumption: single shared collection visible to all staff, no per-document/per-user access restriction in this scope.

**Related:** US-005, US-006, US-007, US-008

### EPIC-003 · approved · source: planning:EPIC-003

Document Chunking and Embedding.

Uploaded documents are automatically split into passages and converted to vector embeddings via Voyage AI (requiring VOYAGE_API_KEY) so relevant content can be retrieved at query time, held in a vector-capable datastore (PostgreSQL + pgvector). Hosting/region left to engineering, no stated data-residency requirement.

**Related:** US-009, US-010

### EPIC-004 · approved · source: planning:EPIC-004

RAG Chat and Grounded Answers.

Staff ask questions through a conversational chat interface; the system retrieves the most relevant passages from the shared knowledge base, sends them with the question to the Anthropic Claude API (requiring ANTHROPIC_API_KEY), and returns an answer grounded in that content with citations back to the originating document/passage.

**Related:** US-011, US-012, US-013, US-014

### EPIC-005 · approved · source: planning:EPIC-005

Per-User Conversation History.

Each staff member's conversations with the chatbot are saved and available to them across sessions. History is private to the individual user; conversation history is saved per user and not shared between users.

**Related:** US-015, US-016

### SPRINT-1 · approved · source: planning:plan:Sprint 1

Sprint 1 (capacity 20): core sign-in, authenticated-only access, and admin-role gating for document management.

Stories: US-001 Sign in with email and password (5 pts, backend-agent); US-002 Restrict access to authenticated users (8 pts, backend-agent) with tasks US-002-T1 auth dependency rejecting unauthenticated API requests (backend-agent), US-002-T2 route guards/sign-in redirects in React (frontend-agent), US-002-T3 tests confirming unauthenticated access rejected on UI and API (qa-agent); US-003 Admin role governs document management (5 pts, backend-agent).

**Related:** US-001, US-002, US-003

### SPRINT-2 · approved · source: planning:plan:Sprint 2

Sprint 2 (capacity 20): session expiry/deactivation, document upload, embedding-key configuration, and knowledge-base listing.

Stories: US-004 End sessions on sign-out and expiry (5 pts, backend-agent); US-005 Upload documents in supported formats (8 pts, backend-agent) with tasks US-005-T1 upload endpoint (backend-agent), US-005-T2 admin upload UI with format validation and status list (frontend-agent), US-005-T3 tests covering accepted/rejected upload formats (qa-agent); US-008 View the shared knowledge base contents (3 pts, frontend-agent); US-010 Configure the embedding provider credential (3 pts, devops-agent).

**Related:** US-004, US-005, US-008, US-010

### SPRINT-3 · approved · source: planning:plan:Sprint 3

Sprint 3 (capacity 20): automatic chunking and embedding pipeline, and document replace.

Stories: US-009 Automatically chunk and embed uploaded content (13 pts, integration-agent) with tasks US-009-T1 text extraction for PDF/Word/text/Markdown (backend-agent), US-009-T2 chunking + Voyage AI embedding pipeline (integration-agent), US-009-T3 Redis queue job processing with status updates (devops-agent), US-009-T4 handle missing/invalid embedding credential and provider errors (backend-agent); US-006 Replace an existing document (5 pts, backend-agent).

**Related:** US-009, US-006

### SPRINT-4 · approved · source: planning:plan:Sprint 4

Sprint 4 (capacity 20): document deletion, conversational chat interface, and per-user saved/private history.

Stories: US-007 Delete a document and remove it from retrieval (5 pts, backend-agent); US-011 Ask a question through the chat interface (8 pts, frontend-agent) with tasks US-011-T1 conversation/message API endpoints (backend-agent), US-011-T2 build responsive chat UI (frontend-agent), US-011-T3 e2e test for asking a question and receiving a reply (qa-agent); US-015 Save conversations per user (3 pts, backend-agent); US-016 Keep conversation history private to the user (3 pts, backend-agent).

**Related:** US-007, US-011, US-015, US-016

### SPRINT-5 · approved · source: planning:plan:Sprint 5

Sprint 5 (capacity 20): relevant-passage retrieval, Claude-grounded answers, and source citations.

Stories: US-012 Retrieve relevant passages for a question (5 pts, integration-agent); US-013 Generate grounded answers with Claude (8 pts, integration-agent) with tasks US-013-T1 Claude API integration for grounded answers (integration-agent), US-013-T2 Claude failure handling (backend-agent), US-013-T3 wire streaming answer delivery into chat UI (frontend-agent); US-014 Cite source documents and passages (5 pts, backend-agent).

**Related:** US-012, US-013, US-014
<!-- sdlc:auto:end narrative -->
