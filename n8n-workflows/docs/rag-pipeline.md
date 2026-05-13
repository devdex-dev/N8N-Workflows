# 🧠 Retrieval-Augmented Generation (RAG) Pipeline

**Status:** ✅ Stable  
**Trigger:** Google Drive file events (created / updated / deleted)  
**File:** [`Retrieval-Augmeted_Generation.json`](../workflows/Retrieval-Augmeted_Generation.json)

---

## Overview

An event-driven RAG pipeline that keeps a **Supabase vector store** in sync with a **Google Drive** folder. Whenever a file is added, modified, or deleted in Drive, the workflow automatically downloads it, chunks the text, generates embeddings via OpenAI, and upserts or removes the vectors from Supabase.

---

## Workflow Diagram

```
Google Drive Events
  ├── FILE CREATED ──► Download file ──► Supabase Vector Store (upsert)
  │                                           │
  │                                    Default Data Loader
  │                                    Embeddings OpenAI
  │                                    Recursive Character Text Splitter
  │
  ├── FILE UPDATED ──► Download file1 ──► Delete a row ──► Re-embed & upsert
  │
  └── FILE DELETED ──► Delete a row1 (remove vectors from Supabase)
```

---

## How It Works

### On File Created
1. Google Drive triggers on new file
2. File is **downloaded**
3. Text is split by **Recursive Character Text Splitter**
4. **OpenAI Embeddings** generates vectors
5. Vectors are **upserted** into Supabase via the Vector Store node

### On File Updated
1. Google Drive triggers on file change
2. **Old vectors are deleted** from Supabase
3. File is re-downloaded and **re-embedded**
4. New vectors are upserted

### On File Deleted
1. Google Drive triggers on file deletion
2. Corresponding vectors are **deleted** from Supabase

---

## Required Credentials

| Credential | Node |
|---|---|
| Google Drive OAuth2 | FILE CREATED · FILE UPDATED · FILE DELETED · Download nodes |
| OpenAI API | Embeddings OpenAI |
| Supabase API | Supabase Vector Store · Delete a row nodes |

---

## Supabase Setup

You need a `documents` table in Supabase with the `pgvector` extension enabled.

```sql
-- Enable pgvector
create extension if not exists vector;

-- Create documents table
create table documents (
  id bigserial primary key,
  content text,
  metadata jsonb,
  embedding vector(1536)
);

-- Create an index for fast similarity search
create index on documents using ivfflat (embedding vector_cosine_ops);
```

---

## Setup Steps

1. Import the workflow JSON
2. Create the Supabase table (see SQL above)
3. Connect **Google Drive OAuth2** — point the trigger to your target folder
4. Connect **OpenAI API** for embeddings
5. Connect **Supabase** credentials — update table name if different
6. Activate the workflow
7. Drop a test file into your Drive folder and verify vectors appear in Supabase

---

## Notes

- Default chunk size depends on `Recursive Character Text Splitter` settings — tune for your use case
- Works best with text-based files (PDF, DOCX, TXT, MD)
- Pair with a Chat/QA workflow that queries the same Supabase vector store for a full RAG system
- The typo in the workflow name (`Retrieval-Augmeted`) is preserved in the filename for compatibility
