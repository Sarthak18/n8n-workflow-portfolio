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

create index on documents using ivfflat (embedding vector_cosine_ops);
Create the match_documents function for similarity search
Copy your Supabase URL and API key
n8n Credentials
Configure the following credentials in n8n:

Supabase API (credential ID: r0fZDlwJlibnAsNl)
Project URL
Service role key
OpenAI API (credential ID: Mu8dewk9miiVVOh0 or fh9h4iTlEZoE4NAU)
API key
Workflow Activation
Import this workflow into your n8n instance
Configure the credentials
Activate the workflow
Access the form URL to upload documents
Use the chat interface to ask questions
Usage
Uploading Documents
Navigate to the form URL (provided after workflow activation)
Select one or more PDF files
Submit the form
Documents will be processed and stored automatically
Asking Questions
Open the chat interface
Type your question about the uploaded documents
The AI agent will:
Search for relevant information in the vector store
Retrieve the top 4 most relevant chunks
Generate a comprehensive answer based on the context
Include document metadata when relevant
Example Use Cases
Legal Document Analysis: Upload contracts or legal documents and ask specific questions
Research Paper Q&A: Upload academic papers and query specific findings or methodologies
Technical Documentation: Upload manuals or guides and get instant answers
Knowledge Base: Build a searchable knowledge base from company documents
Customization Options
Adjust Retrieval Settings
Top K: Change the number of retrieved chunks (default: 4)
Tool Description: Modify how the AI understands when to use retrieval
Embedding Model: Switch to different OpenAI embedding models
Modify AI Behavior
Model: Upgrade to GPT-4 for more complex reasoning
Prompt: Add custom instructions to guide the AI's responses
Temperature: Adjust creativity vs. consistency (in model options)
Form Customization
File Types: Restrict to specific file types beyond PDFs
Multiple Files: Enable/disable multiple file uploads
Form Fields: Add metadata fields (author, category, etc.)
Limitations
Only processes PDF files by default (can be extended to other formats)
Embedding costs scale with document size
Vector search quality depends on chunk size and embedding model
Requires active Supabase and OpenAI connections
Troubleshooting
Documents not being stored
Check Supabase credentials and table schema
Verify the documents table exists with correct columns
Ensure pgvector extension is enabled
Chat not retrieving relevant information
Verify embeddings are being created (check Supabase table)
Increase Top K value for more results
Check that the same embedding model is used for both insert and retrieval
API errors
Verify OpenAI API key is valid and has credits
Check Supabase service role key permissions
Review n8n execution logs for detailed error messages
License
This workflow is provided as-is for use with n8n.

Support
For issues or questions:

n8n Community Forum: https://community.n8n.io/
n8n Documentation: https://docs.n8n.io/

