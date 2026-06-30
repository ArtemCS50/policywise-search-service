# Insurance Policy Assistant

This is a small portfolio project focused on search, backend development, and AI-assisted question answering for insurance policy documents.

The idea is to build two related backend services:

1. **Policy Search Service** — a service for full-text search over insurance policy documents.
2. **Policy AI Assistant** — an AI assistant that answers user questions based on policy documents.

The main goal is not just to build another CRUD application, but to explore how search engines, AI models, vector search, and backend services can work together in a real business-like scenario.

> This project is created for learning and demonstration purposes only. It does not provide real insurance or legal advice.

---

## Why this project?

Insurance policies usually contain a lot of text, rules, exceptions, conditions, and limitations.  
Users often need quick answers to questions like:

- Is water damage covered by my policy?
- What documents are required for a claim?
- Are medical expenses covered during travel?
- What is excluded from the insurance plan?
- Can I cancel my insurance and get a refund?

A simple keyword search is not always enough. Sometimes the user does not know the exact words used in the document. Also, a plain AI model can generate answers that sound correct but are not based on the actual policy.

This project is designed to test two things:

1. How OpenSearch improves document search compared to database-based search.
2. How RAG-based AI answers can be more reliable than plain LLM answers.

---

# Project 1: Policy Search Service

Repository name:

```text
policywise-search-service
```

**Purpose**  
The goal of this service is to provide fast and relevant search over insurance policy documents.  
The service stores policy documents, indexes them into OpenSearch, and exposes REST APIs for searching, filtering, and sorting results.  
It compares traditional database search with OpenSearch full-text search.

**Main features**

- Store insurance policy documents
- Search documents by text query
- Filter by policy type, category, status, or document section
- Sort search results by relevance or date
- Index documents into OpenSearch
- Compare database search with OpenSearch search
- Collect basic search metrics

**Example use cases**  
A user can search:

```text
water damage coverage
```

or:

```text
travel insurance medical expenses
```

or:

```text
claim documents required
```

The service returns the most relevant policy documents or sections.

**Technologies**

- Java
- Spring Boot
- PostgreSQL
- OpenSearch
- Docker Compose
- REST API
- JUnit
- Testcontainers
- Spring Boot Actuator / basic metrics

**Why PostgreSQL and OpenSearch?**  
PostgreSQL is used as the main database and source of truth.  
It stores policy documents, metadata, categories, versions, and document status.  

OpenSearch is used as a search engine.  
It is optimized for full-text search, relevance ranking, fuzzy search, filtering, and fast search queries.  

In a real system, OpenSearch usually does not replace the main database.  
Instead, it works as a search index built from the main data source.

**What I want to measure**  
The project should include simple search experiments and metrics.

**Planned metrics:**

- Average search response time
- P95 search response time
- Indexing time
- Precision@5 for prepared test queries
- Comparison between PostgreSQL search and OpenSearch search

**What I want to demonstrate**  
With this project, I want to show that I can:

- Build a backend search service with Spring Boot
- Work with PostgreSQL as the main data storage
- Integrate OpenSearch for full-text search
- Design REST APIs for search and filtering
- Measure and compare search performance
- Work with Docker-based local infrastructure
- Write clean and testable backend code

# Project 2: Policy AI Assistant

**Repository name:**

```text
policywise-ai-assistant
```

**Purpose**  
The goal of this service is to build an AI assistant that answers questions about insurance policies using provided documents as context.  
The assistant should not answer only from the model’s general knowledge.  
Instead, it should use relevant policy document sections and generate answers based on them.  

This approach is called **RAG** — Retrieval-Augmented Generation.

**What is RAG in this project?**  
RAG means that before the AI model answers a question, the system first searches for relevant document fragments.  

The flow is:

```text
User question
→ Search relevant document chunks
→ Send question + found context to the AI model
→ Generate an answer based on the context
→ Return answer with source references
```

This helps reduce hallucinations and makes the answer more grounded.

**Main features**

- Chat API for user questions
- AI model integration through Spring AI
- Conversation memory using Redis
- Document-based answers using RAG
- Vector search over policy document chunks
- Source references in answers
- Handling out-of-scope questions
- Basic response quality and latency evaluation

**Example questions**  
A user can ask:

```text
Is water damage covered by my home insurance policy?
```

```text
What documents do I need to submit a claim?
```

```text
Does travel insurance cover emergency medical expenses?
```

```text
What exclusions are mentioned in the policy?
```

The assistant should answer based on the available policy documents and mention when the answer cannot be found.

**Technologies**

- Java
- Spring Boot
- Spring AI
- Redis
- Qdrant or pgvector
- Docker Compose
- REST API
- LLM provider: Gemini, OpenAI, Groq, OpenRouter, or Ollama

**Why Redis?**  
Redis is used for conversation memory.  

For example:

```text
User: What does my travel insurance cover?
Assistant: It covers several categories depending on your plan.
User: What about medical expenses?
```

The second question depends on the previous context.  
Redis helps store the chat history by session ID, so the assistant can keep conversation context.

**Why vector database?**  
A vector database is used for semantic search.  
Regular keyword search looks for exact or similar words.  
Vector search looks for meaning.  

For example, a user may ask:

```text
Am I protected if my flight is cancelled?
```

But the document may contain:

```text
Trip interruption and cancellation coverage
```

Vector search can find related content even if the exact words are different.

**What I want to measure**  
The project should include a small evaluation of AI answers.

**Planned metrics:**

- Average response time
- Number of answers with source references
- Grounded answers count
- Out-of-scope refusal rate
- Comparison between plain LLM answers and RAG-based answers
- Approximate token usage or request cost

**What I want to demonstrate**  
With this project, I want to show that I can:

- Integrate AI models into a Spring Boot application
- Use Spring AI for chat-based interactions
- Store conversation memory in Redis
- Use vector search for document retrieval
- Build a simple RAG pipeline
- Reduce unsupported AI answers by using document context
- Evaluate response quality and latency
- Design an AI-powered backend service in a practical domain

## Overall architecture

The two projects are independent, but they are based on the same domain.

```text
policywise-search-service
        |
        | full-text search
        v
PostgreSQL + OpenSearch


policywise-ai-assistant
        |
        | chat + RAG
        v
Redis + Vector Database + LLM
```

Later, the AI assistant can also call the search service to combine keyword search and semantic search.

## What this portfolio project is meant to show

This project is meant to demonstrate backend engineering skills beyond a basic CRUD application.  
It focuses on:

- Backend API design
- Search engine integration
- Database and indexing architecture
- AI model integration
- RAG basics
- Redis-based chat memory
- Dockerized infrastructure
- Simple metrics and experiments
- Clean project structure
- Practical business-like use case

The main idea is to show how a backend engineer can build systems that combine traditional backend development with search and AI capabilities.

## Planned repositories

```text
policywise-search-service
policywise-ai-assistant
```

Each repository should contain:

- Clear README
- Docker Compose setup
- Environment variable examples
- API examples
- Basic tests
- No secrets or credentials
- Simple explanation of architecture and metrics

## Security note

No real credentials should be committed to the repository.  
Use:

```text
.env.example
```

instead of:

```text
.env
```

Do not commit:

- API keys
- Database passwords
- JWT secrets
- Cloud credentials
- Private tokens
