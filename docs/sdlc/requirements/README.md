# Product Requirements

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start narrative -->
## Summary

### REQ-TECH-STACK · approved · source: requirements:technology-stack

Product owner approved a fixed technology stack: FastAPI backend, AWS hosting, PostgreSQL 16 (with pgvector), and React 19 frontend. Chosen to fit a ~50-user internal RAG tool without exotic infrastructure.

- Backend: FastAPI 0.115 -- orchestrates RAG (fetch passages, call Claude, stream cited answer); Python ecosystem has strongest Anthropic/embedding library support; more than sufficient for ~50 users.
- Cloud: AWS -- hosts web app, database, and file storage (S3 for PDFs/Word files) in one account; no data-residency rule stated so region choice left to engineering.
- Database: PostgreSQL 16 with pgvector -- single store for users, roles, conversation history, document metadata, and embeddings, avoiding a separate vector service for a modest document volume.
- Frontend: React 19 -- chat interface with streaming responses plus an admin document-management screen; standard responsive web app since mobile/voice are out of scope.
Design must be against exactly these choices; no substitutions.

### REQ-SERVICES · approved · source: requirements:services

Approved runtime services for the RAG pipeline: Anthropic Claude for answers, Voyage AI for embeddings, pgvector for vector storage, S3 for file storage, Redis as a background queue, and SMTP for account emails.

- AI provider: Anthropic (Claude), SaaS, env ANTHROPIC_API_KEY -- generates grounded, cited replies.
- Embeddings: Voyage AI, SaaS, env VOYAGE_API_KEY -- turns document chunks into vectors for retrieval; Anthropic-recommended partner; OpenAI embeddings noted as a swappable alternative.
- Vector store: pgvector inside PostgreSQL (known/existing) -- avoids a second database for modest volume.
- File storage: Amazon S3, SaaS, env AWS_ACCESS_KEY_ID/AWS_SECRET_ACCESS_KEY/AWS_REGION/S3_BUCKET -- durable storage of original PDFs/Word/text/Markdown files for reprocessing and citation source access.
- Queue: Redis (container redis:7-alpine) -- background chunking/embedding so uploads don't block the admin, and ensures deletions are reliably removed from retrieval.
- Email: SMTP server, SaaS, env SMTP_HOST/SMTP_PORT/SMTP_USER/SMTP_PASSWORD -- password-reset and account-invite messages for email/password sign-in.
Full required API keys/env list: ANTHROPIC_API_KEY, AWS_ACCESS_KEY_ID, AWS_REGION, AWS_SECRET_ACCESS_KEY, REDIS_URL, S3_BUCKET, SMTP_HOST, SMTP_PASSWORD, SMTP_PORT, SMTP_USER, VOYAGE_API_KEY.

### REQ-SCOPE · approved · source: requirements:scope

Delivery scope is lightweight, version 1, approved. In scope: a RAG chat experience with citations and document management; out of scope: native apps, voice input, and automated external connectors.

In scope capabilities: chat interface for asking questions; retrieval of relevant passages from uploaded documents; Claude-generated grounded answers; source citations to originating document/passage; document upload (PDF, Word, text, Markdown); automatic chunking and embedding; shared knowledge base of indexed content; add/replace/delete documents; removal of deleted content from retrieval; per-user saved conversation history; signed-in access for staff; Anthropic Claude for answer generation; Voyage AI for document embeddings.
Out of scope: native mobile/desktop apps, voice input, and automated connectors to external systems (SharePoint, Confluence, Google Drive, website crawling).

### REQ-FEAT-AUTH · approved · source: requirements:Sign-in & accounts

Covers email/password sign-in, sign-out, and admin-only restriction on document management.

- Sign in with email and password: correct credentials open the chat interface; non-matching credentials show an invalid-credentials message without revealing which field was wrong.
- Sign out: ends the session, requiring sign-in again to reach any protected page.
- Admin-only document management: non-admin staff are denied access to the document area but retain chat access; admins can add, replace and delete documents.

### REQ-FEAT-CHAT · approved · source: requirements:Asking questions

Covers submitting questions in chat, retrieving relevant passages, and generating grounded answers via Claude.

- Ask a question in chat: signed-in staff type a question into a conversational interface; the message appears in the thread and an answer is produced in the same conversation.
- Find relevant passages: the system uses document embeddings to retrieve the most relevant passages from the shared knowledge base for the question.
- Generate grounded answers: the question and retrieved passages are sent to the Anthropic Claude API, and the resulting grounded answer is returned in the chat.

### REQ-FEAT-ANSWERS · approved · source: requirements:Answers & citations

Covers honest handling of no-relevant-content cases and displaying source citations on answers.

- Say when nothing relevant is found: when no sufficiently relevant passage exists, the answer states the knowledge base does not contain information on the topic rather than inventing content.
- Show source citations: each generated answer shows citations identifying the originating document and passage(s), clear enough to locate and verify the referenced content.

### REQ-FEAT-HISTORY · approved · source: requirements:Conversation history

Covers saving each user's conversations and letting them view and reopen past conversations.

- Save conversations per user: each staff member's questions and answers are saved; after signing out and back in, previous conversations and answers remain available.
- View and reopen past conversations: a staff member with more than one saved conversation can see their history and reopen each conversation.

### REQ-FEAT-DOCMGMT · approved · source: requirements:Document management

Covers uploading, replacing, deleting, and viewing documents in the shared knowledge base.

- Upload documents in supported formats: PDF, Word (.docx), plain text or Markdown files are accepted, stored and listed with name and status; unsupported formats are rejected with a message stating supported formats.
- Replace an existing document: previous content is removed from retrieval and new content is indexed, so later answers draw on the new version and no longer cite superseded content.
- Delete a document: removes the document and all its chunks/embeddings so content can no longer be retrieved or cited.
- View knowledge base and status: admin sees each document's name, format and status (processing, ready, failed) so failed items can be retried or removed.

### REQ-FEAT-INDEXING · approved · source: requirements:Document indexing

Covers automatic chunking/embedding of documents and secure configuration of the embedding provider key.

- Automatic chunking and embedding: after text extraction, the system splits the document into passages, converts each to an embedding via Voyage AI, and stores embeddings in the vector datastore; the document is marked ready once all passages are stored.
- Configure the embedding provider key: an admin or operator configures the embedding provider API key securely; when not configured, indexing reports that embedding is unavailable instead of marking the document ready.

### NFR-001 · approved · source: requirements:NFR-001

Security: user passwords must be stored only as secure hashes, never in plain text or reversible form.

Category: security. Account passwords are persisted only as a secure hash and are never stored in plain text or any reversible form.

### NFR-002 · approved · source: requirements:NFR-002

Security: only signed-in staff can reach chat or document content; unauthenticated requests are rejected.

Category: security. Anyone not signed in is sent to the sign-in page and cannot see any chat or document pages; application interface requests for knowledge-base content or answers are rejected as unauthenticated.

### NFR-003 · approved · source: requirements:NFR-003

Security: sessions end on sign-out, inactivity timeout, or admin deactivation.

Category: security. A session ends when the user signs out, when it has been inactive beyond the configured session lifetime, and when an admin deactivates the account; the person must sign in again afterward to reach any protected page.

### NFR-004 · approved · source: requirements:NFR-004

Privacy: conversation history is visible only to the staff member who created it, even across browser sessions.

Category: privacy. Saved conversation history is visible only to its owner; a user never sees another user's conversations, including after a different user signs in on the same browser.

### NFR-005 · approved · source: requirements:NFR-005

Usability: the chat interface is responsive and stays usable on smaller screens.

Category: usability. The chat interface is delivered as a responsive web application that adapts to screen size and stays usable on smaller screens.

### NFR-006 · approved · source: requirements:NFR-006

Availability: graceful handling when Claude or the embedding provider is unavailable or errors.

Category: availability. When the Claude answer service is unavailable or errors, the user sees an informative message and their question stays in the conversation to retry. When the embedding provider key is missing or the provider errors, the document is marked failed with an informative status and no partially processed document is treated as ready.

### NFR-007 · approved · source: requirements:NFR-007

Scalability: sized for an internal tool with up to roughly 50 staff and modest document volume, not enterprise scale.

Category: scalability. Target: up to ~50 staff, not enterprise-scale load.

### NFR-008 · approved · source: requirements:NFR-008

Security: service credentials are supplied via secure configuration, not hard-coded; missing embedding key is reported, not silently ignored.

Category: security. The embedding provider, Claude and other service keys are supplied through secure configuration rather than hard-coded; when the embedding key is absent, the system reports that indexing is unavailable instead of silently marking documents ready.

### NFR-009 · approved · source: requirements:NFR-009

Privacy (accepted risk): single shared knowledge base queryable by all staff, with no per-document or per-user access restriction in this scope.

Category: privacy, accepted risk. The knowledge base is a single shared collection that all staff can query; there is no per-document or per-user access restriction in this scope.

### NFR-010 · approved · source: requirements:NFR-010

Compliance (accepted risk): no data-residency or regulatory regime stated for this general-industry internal tool.

Category: compliance, accepted risk. No data-residency requirement and no special regulatory or compliance regime were stated for this general-industry internal tool, so hosting region and compliance controls are left to engineering judgement.
<!-- sdlc:auto:end narrative -->

<!-- sdlc:auto:start summary -->
**Requirements version:** v1  
**Delivery scope:** lightweight  
**Approved:** 2026-11-24

**Capabilities in scope:** Chat interface for asking questions, Retrieval of relevant passages from uploaded documents, Claude-generated answers grounded in the knowledge base, Source citations back to the originating document and passage, Document upload in PDF, Word, text and Markdown, Automatic chunking and embedding of uploaded documents, Shared knowledge base of indexed content, Add, replace and delete documents, Removal of deleted content from retrieval, Per-user saved conversation history, Signed-in access for staff, Anthropic Claude for answer generation, Voyage AI for document embeddings

**Approved technology stack:**

| Layer | Choice | Rationale |
|---|---|---|
| backend | FastAPI | The backend's real job is orchestrating RAG: take a question, fetch matching passages, call Claude, and stream a cited answer back. FastAPI is Python, which is where the Anthropic and embedding libraries are strongest, and it handles the streaming responses a chat experience needs. For a ~50-user internal tool it is more than fast enough. |
| cloud | AWS | An internal tool for 50 staff needs reliable, private hosting for the web app, the database, and document file storage — nothing exotic. AWS provides all of it, including S3 for the uploaded PDFs and Word files, in one account. With no data-residency rule stated, you can simply pick a region near your staff. |
| database | PostgreSQL | You need one store for users, roles, conversation history, document metadata, and — with the pgvector extension — the embeddings that power retrieval. PostgreSQL does all of this in a single database, so you avoid running a separate vector service for a modest document volume. This directly matches the vector-capable datastore the analyst assumed. |
| frontend | React | Your product is a chat interface staff type into and watch answers stream back, plus an admin screen for uploading and managing documents. React handles this live, interactive feel well and is the platform's standard web choice. Since mobile and voice apps are explicitly out of scope, a single responsive web app in React covers every user you described. |
<!-- sdlc:auto:end summary -->

<!-- sdlc:auto:start epics -->
## EPIC-001: Staff Authentication and Role-Based Access

All access to the chatbot is restricted to signed-in staff. Users authenticate with email and password accounts, there is no public or anonymous access, and an admin role governs who can manage documents. Session and sign-in security are expressed as outcomes the user or admin can verify.

### US-001: Sign in with email and password

**Priority:** critical

> As a member of staff, I want to sign in with my email address and password, so that I can access the internal knowledge base chatbot.

| ID | Given | When | Then |
|---|---|---|---|
| AC-001 | I am a registered user on the sign-in page | I enter my correct email and password and submit | I am signed in and taken to the chat interface |
| AC-002 | I am on the sign-in page | I enter an email or password that does not match an account | I am not signed in and I see a message that the credentials are invalid, without revealing which field was wrong |
| AC-003 | the product stores account credentials | a user's password is persisted | it is stored only as a secure hash and never in plain text or in a reversible form |

### US-002: Restrict access to authenticated users

**Priority:** critical

> As a organisation, I want only signed-in staff to reach the chatbot and documents, so that internal knowledge is not exposed to anonymous or public visitors.

| ID | Given | When | Then |
|---|---|---|---|
| AC-004 | I am not signed in | I try to open the chat interface or any document management page | I am redirected to the sign-in page and cannot see any internal content |
| AC-005 | I am not signed in | I call an application interface endpoint that returns knowledge-base content or answers | the request is rejected as unauthenticated |

### US-003: Admin role governs document management

**Priority:** high

> As a administrator, I want document management to be limited to the admin role, so that only trusted users can change the shared knowledge base while all staff can still query it.

| ID | Given | When | Then |
|---|---|---|---|
| AC-006 | I am signed in as a staff user without the admin role | I attempt to open or act on the document management area | access is denied and I remain able to use the chat interface |
| AC-007 | I am signed in as an admin | I open the document management area | I can add, replace and delete documents |

### US-004: End sessions on sign-out and expiry

**Priority:** high

> As a member of staff, I want my session to end when I sign out, expire, or my account is deactivated, so that access cannot continue after it should have stopped.

| ID | Given | When | Then |
|---|---|---|---|
| AC-008 | I am signed in | I choose to sign out | my session ends and I must sign in again to reach any protected page |
| AC-009 | I have been signed in and inactive beyond the configured session lifetime | I next make a request | my session is treated as expired and I must sign in again |
| AC-010 | an admin deactivates my account | I next make a request on any existing session | my session is no longer valid and I cannot access the application |

## EPIC-002: Document Management and Knowledge Base

Admins build and maintain a single shared knowledge base by uploading documents in PDF, Word (.docx), plain text and Markdown. They can add, replace and delete documents, and deleting a document removes its content from retrieval so stale information is no longer used to answer questions. Note (assumption): the knowledge base is a single shared collection visible to all staff; there is no per-document or per-user access restriction in this scope.

### US-005: Upload documents in supported formats

**Priority:** critical

> As a administrator, I want to upload documents in PDF, Word, plain text and Markdown, so that their content becomes part of the shared knowledge base.

| ID | Given | When | Then |
|---|---|---|---|
| AC-011 | I am signed in as an admin on the document management area | I upload a PDF, .docx, .txt or .md file | the document is accepted, stored, and listed in the knowledge base with its name and status |
| AC-012 | I am uploading a document | I select a file whose format is not one of the supported types | the upload is rejected with a message stating the supported formats |
| AC-013 | I have uploaded a document | its text extraction and indexing complete | its status shows it is ready for retrieval |

### US-006: Replace an existing document

**Priority:** high

> As a administrator, I want to replace a document with an updated version, so that the knowledge base reflects the latest information without creating duplicates.

| ID | Given | When | Then |
|---|---|---|---|
| AC-014 | a document already exists in the knowledge base | I replace it with a new file | the previous content is removed from retrieval and the new content is chunked, embedded and made available |
| AC-015 | I have replaced a document | a staff member next asks a question that the new version covers | answers draw on the new content and no longer cite the superseded content |

### US-007: Delete a document and remove it from retrieval

**Priority:** high

> As a administrator, I want to delete a document from the knowledge base, so that its content is no longer used to answer questions.

| ID | Given | When | Then |
|---|---|---|---|
| AC-016 | a document exists and has been indexed for retrieval | I delete it | the document and all of its chunks and embeddings are removed so it can no longer be retrieved |
| AC-017 | I have deleted a document | a staff member asks a question the deleted document previously answered | the deleted content is not retrieved and is not cited in any answer |

### US-008: View the shared knowledge base contents

**Priority:** medium

> As a administrator, I want to see the list of documents currently in the knowledge base and their indexing status, so that I can manage what content is available to staff.

| ID | Given | When | Then |
|---|---|---|---|
| AC-018 | documents have been uploaded | I open the document management area | I see each document's name, format and current status (for example processing, ready, or failed) |
| AC-019 | a document's indexing has failed | I view the list | the document is shown with a failed status so I can retry or remove it |

## EPIC-003: Document Chunking and Embedding

Uploaded documents are automatically split into passages (chunks) and converted to vector embeddings so relevant content can be retrieved at query time. Embeddings are produced using Voyage AI and held in a vector-capable datastore. Assumptions to surface: embeddings are generated via an external embedding provider (Voyage AI) that requires its API key (e.g. VOYAGE_API_KEY); a vector-capable datastore such as PostgreSQL with pgvector holds the embeddings; hosting and region are left to engineering with no stated data-residency requirement.

### US-009: Automatically chunk and embed uploaded content

**Priority:** critical

> As a administrator, I want uploaded documents to be automatically chunked and embedded, so that their content can be retrieved to ground chatbot answers without manual preparation.

| ID | Given | When | Then |
|---|---|---|---|
| AC-020 | I upload a supported document | the system has extracted its text | the text is split into passages and each passage is converted to an embedding and stored in the vector datastore |
| AC-021 | the embedding provider returns embeddings for a document's passages | all passages are stored | the document is marked ready for retrieval |
| AC-022 | the embedding provider (Voyage AI) key is missing or the provider returns an error | the system attempts to embed a document | the document is marked failed with an informative status and no partial document is treated as ready |

### US-010: Configure the embedding provider credential

**Priority:** high

> As a administrator or operator, I want the embedding provider API key to be configured securely, so that the system can generate embeddings for retrieval.

| ID | Given | When | Then |
|---|---|---|---|
| AC-023 | the embedding provider API key is set to a value such as <your-voyage-api-key> | a document is uploaded | the system successfully requests embeddings from Voyage AI |
| AC-024 | the embedding provider API key is not configured | an admin attempts to upload and index a document | the system reports that embedding is unavailable until the key is configured, rather than silently marking the document ready |

## EPIC-004: RAG Chat and Grounded Answers

Staff ask questions through a conversational chat interface. The system retrieves the most relevant passages from the shared knowledge base, sends them with the question to the Anthropic Claude API, and returns an answer grounded in that content with citations back to the originating document and passage. Assumptions to surface: answer generation uses the Anthropic Claude API and requires its API key (ANTHROPIC_API_KEY); answers cite the source documents/passages they draw from.

### US-011: Ask a question through the chat interface

**Priority:** critical

> As a member of staff, I want to ask questions in a conversational chat interface, so that I can get answers from the organisation's knowledge base.

| ID | Given | When | Then |
|---|---|---|---|
| AC-025 | I am signed in | I type a question and submit it | my message appears in the conversation and the system produces an answer in the same thread |
| AC-026 | I am using the chat interface on a smaller screen | I open the application in a responsive web browser | the chat interface adapts to the screen size and remains usable |

### US-012: Retrieve relevant passages for a question

**Priority:** critical

> As a member of staff, I want the system to find the passages most relevant to my question, so that the answer is based on the right content from the knowledge base.

| ID | Given | When | Then |
|---|---|---|---|
| AC-027 | indexed documents exist and I ask a question | the system processes my question | it retrieves the most relevant passages from the shared knowledge base using their embeddings |
| AC-028 | no document in the knowledge base is relevant to my question | the system finds no sufficiently relevant passages | the answer states that the knowledge base does not contain information on the topic rather than inventing content |

### US-013: Generate grounded answers with Claude

**Priority:** critical

> As a member of staff, I want answers generated by Claude using the retrieved passages, so that my answers are grounded in our documents and not fabricated.

| ID | Given | When | Then |
|---|---|---|---|
| AC-029 | relevant passages have been retrieved for my question | the system generates an answer | it sends the question and retrieved passages to the Anthropic Claude API and returns the resulting answer in the chat |
| AC-030 | the Anthropic Claude API key is set to a value such as <your-anthropic-api-key> | I ask a question | the request to Claude succeeds and an answer is returned |
| AC-031 | the Claude API is unavailable or returns an error | I ask a question | I see an informative message that an answer could not be generated and my question remains in the conversation so I can retry |

### US-014: Cite source documents and passages

**Priority:** high

> As a member of staff, I want each answer to cite the documents and passages it came from, so that I can verify the answer against the source.

| ID | Given | When | Then |
|---|---|---|---|
| AC-032 | an answer was generated from retrieved passages | the answer is displayed | it shows citations identifying the originating document and the passage(s) the answer drew on |
| AC-033 | an answer includes citations | I inspect a citation | it identifies the source document clearly enough for me to locate the referenced content |

## EPIC-005: Per-User Conversation History

Each staff member's conversations with the chatbot are saved and available to them across sessions. History is private to the individual user. Assumption to surface: conversation history is saved per user and is not shared between users.

### US-015: Save conversations per user

**Priority:** high

> As a member of staff, I want my chat conversations to be saved, so that I can return to earlier questions and answers.

| ID | Given | When | Then |
|---|---|---|---|
| AC-034 | I have asked questions and received answers in a conversation | I sign out and sign back in | my previous conversation and its answers are still available to me |
| AC-035 | I have more than one conversation | I view my history | I can see and reopen each saved conversation |

### US-016: Keep conversation history private to the user

**Priority:** high

> As a member of staff, I want my conversation history to be visible only to me, so that my questions are not exposed to other staff.

| ID | Given | When | Then |
|---|---|---|---|
| AC-036 | another staff member has their own conversation history | I view my history | I see only my own conversations and not those of other users |
| AC-037 | my account's session ends | another user signs in on the same browser | they cannot see my saved conversations |
<!-- sdlc:auto:end epics -->
