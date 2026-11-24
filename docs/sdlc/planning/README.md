# Planning

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start narrative -->
### EPIC-001 · approved · source: planning:EPIC-001

All access to the chatbot is restricted to signed-in staff via email/password accounts, with no public or anonymous access, and an admin role governing document management.

Session and sign-in security are expressed as outcomes the user or admin can verify (sign-in, redirect of unauthenticated users, admin-only document access, session end on sign-out/expiry/deactivation).

**Related:** US-001, US-002, US-003, US-004

### EPIC-002 · approved · source: planning:EPIC-002

Admins build and maintain a single shared knowledge base by uploading PDF, Word, text and Markdown documents, and can add, replace and delete documents.

Deleting a document removes its content from retrieval so stale information is no longer used to answer questions. Assumption noted: the knowledge base is a single shared collection visible to all staff with no per-document or per-user access restriction in this scope.

**Related:** US-005, US-006, US-007, US-008

### EPIC-003 · approved · source: planning:EPIC-003

Uploaded documents are automatically split into passages and converted to vector embeddings via Voyage AI so relevant content can be retrieved at query time.

Embeddings are held in a vector-capable datastore (PostgreSQL with pgvector). Assumptions surfaced: embeddings require the Voyage AI API key (VOYAGE_API_KEY); hosting and region are left to engineering with no stated data-residency requirement.

**Related:** US-009, US-010

### EPIC-004 · approved · source: planning:EPIC-004

Staff ask questions through a conversational chat interface; the system retrieves the most relevant passages, sends them with the question to Claude, and returns a grounded answer with citations.

Assumptions surfaced: answer generation uses the Anthropic Claude API and requires ANTHROPIC_API_KEY; answers cite the source documents/passages they draw from.

**Related:** US-011, US-012, US-013, US-014

### EPIC-005 · approved · source: planning:EPIC-005

Each staff member's conversations with the chatbot are saved and available to them across sessions, and history is private to the individual user.

Assumption surfaced: conversation history is saved per user and is not shared between users.

**Related:** US-015, US-016
<!-- sdlc:auto:end narrative -->
