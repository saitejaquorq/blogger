# Planning

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start narrative -->
### EPIC-001 · approved · source: planning:EPIC-001

Restricts all chatbot access to signed-in staff via email/password accounts, with no public or anonymous access, and gates document management behind an admin role.

Goal: all access to the chatbot is restricted to signed-in staff; session and sign-in security and admin-role gating are expressed as user/admin-verifiable outcomes. Stories: US-001 (sign in with email/password), US-002 (restrict access to authenticated users), US-003 (admin role governs document management), US-004 (end sessions on sign-out and expiry).

**Related:** US-001, US-002, US-003, US-004

### EPIC-002 · approved · source: planning:EPIC-002

Admins build and maintain a single shared knowledge base by uploading, replacing, and deleting documents in PDF, Word, plain text, and Markdown formats.

Goal: admins manage a single shared document collection; deleting a document removes its content from retrieval so stale information stops being used. Assumption noted: the knowledge base has no per-document or per-user access restriction in this scope. Stories: US-005 (upload supported formats), US-006 (replace an existing document), US-007 (delete a document and remove it from retrieval), US-008 (view knowledge base contents and status).

**Related:** US-005, US-006, US-007, US-008

### EPIC-003 · approved · source: planning:EPIC-003

Uploaded documents are automatically chunked and converted into vector embeddings via Voyage AI, stored in a vector-capable datastore, so content can be retrieved at query time.

Goal: automatic chunking and embedding without manual preparation, using Voyage AI as the embedding provider and a vector-capable datastore (e.g. PostgreSQL with pgvector). Assumptions surfaced: requires an embedding-provider API key (e.g. VOYAGE_API_KEY); hosting/region left to engineering with no stated data-residency requirement. Stories: US-009 (automatically chunk and embed uploaded content), US-010 (configure the embedding provider credential).

**Related:** US-009, US-010

### EPIC-004 · approved · source: planning:EPIC-004

Staff ask questions in a conversational chat interface; the system retrieves relevant passages from the knowledge base, sends them with the question to the Anthropic Claude API, and returns a grounded, cited answer.

Goal: deliver retrieval-augmented answers grounded in the shared knowledge base with citations back to source documents and passages. Assumptions surfaced: answer generation uses the Anthropic Claude API and requires ANTHROPIC_API_KEY; answers must cite the documents/passages they draw from. Stories: US-011 (ask a question through the chat interface), US-012 (retrieve relevant passages for a question), US-013 (generate grounded answers with Claude), US-014 (cite source documents and passages).

**Related:** US-011, US-012, US-013, US-014

### EPIC-005 · approved · source: planning:EPIC-005

Each staff member's conversations are saved and available to them across sessions, with history kept private to the individual user.

Goal: persist per-user conversation history and keep it private, not shared between users. Assumption surfaced: conversation history is saved per user and is not shared between users. Stories: US-015 (save conversations per user), US-016 (keep conversation history private to the user).

**Related:** US-015, US-016

### Sprint 1 · approved · source: planning:Sprint 1

First sprint (capacity 20) stands up authentication, session lifecycle, and admin-role gating so all later features can sit behind sign-in.

Goal: 'Stand up authentication, session lifecycle and admin-role gating so every later feature can sit behind sign-in.' Capacity: 20 points. Story-to-sprint mapping is inferred from the goal text, which corresponds to EPIC-001's stories US-001 (5 pts, backend-agent), US-002 (5 pts, backend-agent), US-003 (3 pts, backend-agent), US-004 (5 pts, backend-agent) -- totaling 18 of the 20-point capacity; no explicit sprint assignment table was provided in the input.

**Related:** EPIC-001, US-001, US-002, US-003, US-004

### Sprint 2 · approved · source: planning:Sprint 2

Second sprint (capacity 20) enables document upload, secure embedding-provider configuration, and visibility into the knowledge base.

Goal: 'Enable document upload, secure embedding-provider configuration, and visibility into the knowledge base.' Capacity: 20 points. Inferred from goal text: US-005 upload (8 pts, backend-agent, with tasks US-005-T1/T2/T3), US-008 view knowledge base contents (3 pts, frontend-agent), US-010 configure embedding provider credential (3 pts, devops-agent) -- totaling 14 of 20 points. No explicit sprint assignment table was provided in the input.

**Related:** EPIC-002, EPIC-003, US-005, US-008, US-010

### Sprint 3 · approved · source: planning:Sprint 3

Third sprint (capacity 20) automatically chunks and embeds documents, and supports replacing/deleting content safely.

Goal: 'Automatically chunk and embed documents, and support replacing/deleting content safely.' Capacity: 20 points. Inferred from goal text: US-009 automatic chunking/embedding (8 pts, backend-agent, with tasks US-009-T1/T2/T3), US-006 replace a document (5 pts, backend-agent), US-007 delete a document (5 pts, backend-agent) -- totaling 18 of 20 points. No explicit sprint assignment table was provided in the input.

**Related:** EPIC-002, EPIC-003, US-006, US-007, US-009

### Sprint 4 · approved · source: planning:Sprint 4

Fourth sprint (capacity 20) delivers the end-to-end RAG chat experience: retrieval, Claude-grounded answers, and citations.

Goal: 'Deliver the end-to-end RAG chat experience: retrieval, Claude-grounded answers and citations.' Capacity: 20 points. Inferred from goal text: US-011 ask a question (5 pts, frontend-agent), US-012 retrieve relevant passages (5 pts, backend-agent), US-013 generate grounded answers with Claude (5 pts, integration-agent), US-014 cite source documents and passages (3 pts, frontend-agent) -- totaling 18 of 20 points. No explicit sprint assignment table was provided in the input.

**Related:** EPIC-004, US-011, US-012, US-013, US-014

### Sprint 5 · approved · source: planning:Sprint 5

Fifth sprint (capacity 20) persists and protects each user's conversation history.

Goal: 'Persist and protect each user's conversation history.' Capacity: 20 points. Inferred from goal text: US-015 save conversations per user (5 pts, backend-agent), US-016 keep conversation history private to the user (3 pts, backend-agent) -- totaling 8 of 20 points. No explicit sprint assignment table was provided in the input.

**Related:** EPIC-005, US-015, US-016
<!-- sdlc:auto:end narrative -->
