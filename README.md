# EvalForge

**EvalForge** is an evaluation-centric Retrieval-Augmented Generation (RAG) platform.

It combines:

- **Python + FastAPI** for document ingestion, embeddings, vector search, and RAG pipelines
- **Chroma** as the vector database
- **OpenAI (or compatible) models** for embeddings and LLM answers
- **Java + Spring Boot + PostgreSQL** for metadata, experiment tracking, and evaluation dashboards

EvalForge is built to showcase **AI engineering** skills:
- RAG architecture
- Retrieval & generation quality evaluation
- Experiment tracking and configuration management
- Latency and performance measurement
- Clean separation between AI services and application backend

---

## High-level Features

- **Document ingestion**
  - Upload documents (via Java gateway)
  - Text extraction and chunking
  - Embedding generation via OpenAI (or other providers)
  - Persistent vector storage in Chroma

- **RAG query pipeline**
  - Embed user questions
  - Vector search with metadata filtering
  - LLM answer generation using retrieved context
  - Answers with associated context chunks and metadata

- **Evaluation / Experiments**
  - Labeled question–answer pairs for evaluation
  - Automated evaluation runs:
    - Retrieval metrics (e.g., Recall@k)
    - Answer quality scoring (LLM-as-judge or heuristic)
    - Latency measurements
  - Experiment runs stored in PostgreSQL and exposed via Spring Boot APIs
  - Comparison of different configurations (chunk size, top-k, model choice, etc.)

---

## Architecture Overview

EvalForge is split into two main backends:

1. **Python RAG Service (FastAPI)**
   - Ingestion: chunking, embeddings, Chroma indexing
   - Query: retrieval + LLM answer generation
   - Evaluation logic: given a set of labeled questions, executes RAG and computes metrics

2. **Java Gateway & Eval Service (Spring Boot)**
   - User/auth management (optional for MVP)
   - Document metadata lifecycle
   - Evaluation dataset management (questions + ground truths)
   - Coordination of evaluation runs and storage of results in PostgreSQL
   - API surface for a dashboard/UI to visualize metrics

### Key Components

- **Chroma Vector DB**
  - Stores chunks, embeddings, and metadata
  - Provides similarity search with metadata filters

- **PostgreSQL**
  - Stores:
    - Users
    - Documents metadata
    - Evaluation questions
    - Evaluation runs and their results

- **LLM Provider**
  - Embedding model (e.g., `text-embedding-3-small`)
  - Chat/completion model (e.g., `gpt-4.1-mini`, configurable)

---

## Tech Stack

**Python / RAG Service**
- Python 3.10+
- FastAPI
- ChromaDB
- OpenAI (or OpenAI-compatible client)
- Pydantic

**Java / Gateway & Eval**
- Java 17+
- Spring Boot
- Spring Web / Spring MVC
- Spring Data JPA
- PostgreSQL

**Infrastructure (local dev)**
- Docker / Docker Compose (for Postgres, optional for Chroma)
- `.env` configuration for API keys and paths

---

## Repository Structure (Planned)

Example structure:

```text
EvalForge/
  python-rag/
    app/
      api/
      core/
      rag/
      schemas/
    requirements.txt

  java-gateway/
    src/main/java/...
    src/main/resources/...
    pom.xml

  docker/
    docker-compose.yml  # Postgres, (optionally) Chroma, etc.

  docs/
    architecture.md
    eval-design.md

  README.md
