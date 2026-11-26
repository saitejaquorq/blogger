# Planning

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start narrative -->
### EPIC-001 · approved · source: planning:EPIC-001

Restrict all chatbot access to signed-in staff using email/password accounts, with an admin role governing who can manage documents, and verifiable session/sign-in security outcomes.

Goal: no public or anonymous access; authentication via email and password; admin role gates document management; session and sign-in security expressed as user/admin-verifiable outcomes.

**Related:** US-001, US-002, US-003, US-004

### EPIC-002 · approved · source: planning:EPIC-002

Admins build and maintain a single shared knowledge base by uploading, replacing and deleting documents in PDF, Word, plain text and Markdown; deletions remove content from retrieval immediately.

Goal: single shared knowledge-base collection visible to all staff with no per-document or per-user access restriction (stated assumption). Supported formats: PDF, .docx, .txt, .md.

**Related:** US-005, US-006, US-007, US-008

### EPIC-003 · approved · source: planning:EPIC-003

Automatically split uploaded documents into passages and convert them to vector embeddings so relevant content can be retrieved at query time.

Goal: embeddings generated via Voyage AI (requires VOYAGE_API_KEY) and stored in a vector-capable datastore such as PostgreSQL with pgvector. Hosting/region left to engineering; no stated data-residency requirement (assumption).

**Related:** US-009, US-010

### EPIC-004 · approved · source: planning:EPIC-004

Staff ask questions in a conversational chat interface; the system retrieves relevant passages from the shared knowledge base and sends them with the question to the Anthropic Claude API to produce a grounded, cited answer.

Goal: answer generation uses the Anthropic Claude API (requires ANTHROPIC_API_KEY, placeholder only); answers cite the source documents/passages they draw from (stated assumptions).

**Related:** US-011, US-012, US-013, US-014

### EPIC-005 · approved · source: planning:EPIC-005

Each staff member's conversations with the chatbot are saved and available to them across sessions, with history kept private to the individual user.

Goal: conversation history is saved per user and is not shared between users (stated assumption).

**Related:** US-015, US-016

### SPRINT-1 · approved · source: planning:Sprint 1

Sprint 1 stands up sign-in, sessions, admin RBAC and the Voyage embedding credential so later features have a secure, configured base.

Goal: 'Stand up sign-in, sessions, admin RBAC and the Voyage embedding credential so every later feature has a secure, configured base to build on.' Capacity: 20 points. Targets sign-in (US-001), access restriction (US-002), admin RBAC (US-003), session end/expiry foundation (US-004), and embedding provider credential configuration (US-010).

**Related:** US-001, US-002, US-003, US-004, US-010

### SPRINT-2 · approved · source: planning:Sprint 2

Sprint 2 delivers document upload/storage with listing, plus the conversational chat UI and its message endpoints.

Goal: 'Deliver document upload/storage with listing, and the conversational chat UI with its message endpoints.' Capacity: 20 points. Targets document upload (US-005), knowledge-base listing (US-008), and the chat interface (US-011).

**Related:** US-005, US-008, US-011

### SPRINT-3 · approved · source: planning:Sprint 3

Sprint 3 builds the automatic chunking-and-embedding pipeline and completes session expiry/deactivation behaviour.

Goal: 'Build the automatic chunking-and-embedding pipeline and complete session expiry/deactivation behaviour.' Capacity: 20 points. Targets chunking/embedding (US-009) and completes session expiry/deactivation behaviour (US-004, continued from Sprint 1).

**Related:** US-009, US-004

### SPRINT-4 · approved · source: planning:Sprint 4

Sprint 4 implements passage retrieval and Claude-grounded answer generation, and ensures deleted documents are removed from retrieval.

Goal: 'Implement passage retrieval and Claude-grounded answer generation, and remove deleted documents from retrieval.' Capacity: 20 points. Targets passage retrieval (US-012), grounded answer generation (US-013), and deletion/retrieval removal (US-007).

**Related:** US-012, US-013, US-007

### SPRINT-5 · approved · source: planning:Sprint 5

Sprint 5 adds source citations, per-user conversation history with privacy enforcement, and document replace.

Goal: 'Add source citations, per-user conversation history with privacy enforcement, and document replace.' Capacity: 20 points. Targets citations (US-014), per-user history (US-015), history privacy (US-016), and document replace (US-006).

**Related:** US-014, US-015, US-016, US-006
<!-- sdlc:auto:end narrative -->
