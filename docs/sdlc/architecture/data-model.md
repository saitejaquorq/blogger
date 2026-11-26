# Data Model

<!-- sdlc:auto:start entities -->
### users

| Field | Type | PK | FK | Nullable |
|---|---|---|---|---|
| id | uuid | True | — | False |
| email | citext | False | — | False |
| password_hash | text | False | — | False |
| role | text | False | — | False |
| is_active | boolean | False | — | False |
| created_at | timestamptz | False | — | False |

### sessions

| Field | Type | PK | FK | Nullable |
|---|---|---|---|---|
| id | uuid | True | — | False |
| user_id | uuid | False | users.id | False |
| token_hash | text | False | — | False |
| expires_at | timestamptz | False | — | False |
| last_active_at | timestamptz | False | — | False |
| revoked_at | timestamptz | False | — | True |
| created_at | timestamptz | False | — | False |

### documents

| Field | Type | PK | FK | Nullable |
|---|---|---|---|---|
| id | uuid | True | — | False |
| name | text | False | — | False |
| format | text | False | — | False |
| s3_key | text | False | — | False |
| status | text | False | — | False |
| status_detail | text | False | — | True |
| uploaded_by | uuid | False | users.id | False |
| created_at | timestamptz | False | — | False |
| updated_at | timestamptz | False | — | False |

### document_chunks

| Field | Type | PK | FK | Nullable |
|---|---|---|---|---|
| id | uuid | True | — | False |
| document_id | uuid | False | documents.id | False |
| chunk_index | integer | False | — | False |
| content | text | False | — | False |
| embedding | vector(1024) | False | — | False |
| created_at | timestamptz | False | — | False |

### conversations

| Field | Type | PK | FK | Nullable |
|---|---|---|---|---|
| id | uuid | True | — | False |
| user_id | uuid | False | users.id | False |
| title | text | False | — | True |
| created_at | timestamptz | False | — | False |
| updated_at | timestamptz | False | — | False |

### messages

| Field | Type | PK | FK | Nullable |
|---|---|---|---|---|
| id | uuid | True | — | False |
| conversation_id | uuid | False | conversations.id | False |
| role | text | False | — | False |
| content | text | False | — | False |
| created_at | timestamptz | False | — | False |

### message_citations

| Field | Type | PK | FK | Nullable |
|---|---|---|---|---|
| id | uuid | True | — | False |
| message_id | uuid | False | messages.id | False |
| document_id | uuid | False | documents.id | False |
| chunk_id | uuid | False | document_chunks.id | True |
| snippet | text | False | — | True |
<!-- sdlc:auto:end entities -->

<!-- sdlc:auto:start relationships -->
| Source | Target | Type |
|---|---|---|
| users | sessions | one-to-many |
| users | documents | one-to-many |
| users | conversations | one-to-many |
| documents | document_chunks | one-to-many |
| conversations | messages | one-to-many |
| messages | message_citations | one-to-many |
| documents | message_citations | one-to-many |
<!-- sdlc:auto:end relationships -->

<!-- sdlc:auto:start er-diagram -->
```mermaid
erDiagram
  users {
    uuid id PK
    citext email
    text password_hash
    text role
    boolean is_active
    timestamptz created_at
  }
  sessions {
    uuid id PK
    uuid user_id FK
    text token_hash
    timestamptz expires_at
    timestamptz last_active_at
    timestamptz revoked_at
    timestamptz created_at
  }
  documents {
    uuid id PK
    text name
    text format
    text s3_key
    text status
    text status_detail
    uuid uploaded_by FK
    timestamptz created_at
    timestamptz updated_at
  }
  document_chunks {
    uuid id PK
    uuid document_id FK
    integer chunk_index
    text content
    vector_1024_ embedding
    timestamptz created_at
  }
  conversations {
    uuid id PK
    uuid user_id FK
    text title
    timestamptz created_at
    timestamptz updated_at
  }
  messages {
    uuid id PK
    uuid conversation_id FK
    text role
    text content
    timestamptz created_at
  }
  message_citations {
    uuid id PK
    uuid message_id FK
    uuid document_id FK
    uuid chunk_id FK
    text snippet
  }
  users ||--o{ sessions : "one-to-many"
  users ||--o{ documents : "one-to-many"
  users ||--o{ conversations : "one-to-many"
  documents ||--o{ document_chunks : "one-to-many"
  conversations ||--o{ messages : "one-to-many"
  messages ||--o{ message_citations : "one-to-many"
  documents ||--o{ message_citations : "one-to-many"
```
<!-- sdlc:auto:end er-diagram -->
