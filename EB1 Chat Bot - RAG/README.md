This n8n workflow implements a RAG (Retrieval-Augmented Generation) chatbot that allows users to upload PDF documents and then ask questions about their content. The AI agent retrieves relevant information from the uploaded documents to provide accurate, context-aware answers.

## How It Works

### 1. Document Upload Flow
- **Form Trigger**: Users submit PDF files through a web form
- **Vector Store**: Documents are processed and stored in Supabase with embeddings
  - PDFs are automatically loaded and split into chunks
  - Text chunks are converted to embeddings using OpenAI's `text-embedding-3-small` model
  - Embeddings are stored in Supabase for efficient similarity search

### 2. Chat Flow
- **Chat Trigger**: Users send questions through the chat interface
- **AI Agent**: Powered by GPT-4.1-mini with retrieval capabilities
  - Receives user questions
  - Uses the retrieval tool to search the vector store for relevant document chunks
  - Generates answers based on retrieved context
  - Returns responses directly in the chat

## Components

### Triggers
- **On form submission**: Web form for uploading PDF files (supports multiple files)
- **When chat message received**: Built-in chat interface for asking questions

### Vector Storage
- **Supabase Vector Store**: PostgreSQL database with pgvector extension
  - Table: `documents`
  - Query function: `match_documents`
  - Top K results: 4 most relevant chunks per query

### AI Components
- **OpenAI Chat Model**: GPT-4.1-mini for natural language understanding and generation
- **Embeddings**: OpenAI text-embedding-3-small for semantic search
- **Retrieval Tool**: Vector similarity search with document metadata

## Setup Instructions

### Prerequisites
1. n8n instance (cloud or self-hosted)
2. Supabase account with a project
3. OpenAI API key (or use n8n free credits)

### Supabase Setup
1. Create a new Supabase project
2. Enable the `pgvector` extension
3. Create the `documents` table with the required schema:
```sql
create table documents (
  id bigserial primary key,
  content text,
  metadata jsonb,
  embedding vector(1536)
);
