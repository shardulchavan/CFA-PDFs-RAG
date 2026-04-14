# CFA-PDFs-RAG 📄

> A Retrieval-Augmented Generation (RAG) pipeline that extracts text from CFA exam PDFs and answers user questions using OpenAI — orchestrated with Airflow and served via FastAPI + Streamlit.

![Tech Stack](https://img.shields.io/badge/Python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=for-the-badge&logo=openai&logoColor=white)
![Airflow](https://img.shields.io/badge/Apache%20Airflow-017CEE?style=for-the-badge&logo=apache-airflow&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-005571?style=for-the-badge&logo=fastapi)
![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white)
![Pinecone](https://img.shields.io/badge/Pinecone-000000?style=for-the-badge)
![Docker](https://img.shields.io/badge/Docker-0db7ed?style=for-the-badge&logo=docker&logoColor=white)

---

## What it does

CFA-PDFs-RAG is a question-answering application built on top of CFA Institute study materials. Users upload or select a PDF, and the system extracts its content, chunks and embeds it into a vector store (Pinecone), and uses OpenAI to answer natural-language questions grounded in the document — no hallucinations, only answers from the source material.

Key capabilities:
- **Automated PDF extraction** — Airflow pipeline ingests and processes CFA PDFs on a schedule
- **Chunking & embedding** — document text is split into semantic chunks and stored as vector embeddings in Pinecone
- **RAG-based Q&A** — user questions are embedded, matched against relevant chunks, and answered by GPT with source context
- **FastAPI backend** — REST API handles document ingestion, query processing, and response streaming
- **Streamlit frontend** — clean UI for uploading PDFs, asking questions, and viewing sourced answers

---

## Architecture

```
PDF Source
    │
    ▼
Airflow DAG (scheduled extraction)
    │
    ├──► Text Extraction (PyPDF / pdfminer)
    │
    ├──► Chunking + OpenAI Embeddings
    │
    └──► Pinecone Vector Store
                │
                ▼
         User Query (Streamlit UI)
                │
                ▼
         FastAPI → Pinecone similarity search
                │
                ▼
         OpenAI GPT (answer grounded in retrieved chunks)
                │
                ▼
         Answer + Source References → UI
```
![image](Architecture_diagram.png)

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Streamlit |
| Backend API | FastAPI |
| Orchestration | Apache Airflow |
| LLM | OpenAI GPT-3.5 / GPT-4 |
| Vector Store | Pinecone |
| PDF Extraction | PyPDF2 / pdfminer |
| Containerization | Docker |

---

## My Contributions

This was a 3-person team project. My primary ownership areas:

- **Streamlit frontend** — built the full Q&A interface including PDF upload, question input, answer rendering with source citations, and session history
- **RAG query pipeline** — implemented the end-to-end retrieval flow: user query → embedding → Pinecone search → context assembly → OpenAI completion
- **FastAPI endpoints** — designed and built the `/query` and `/ingest` API routes
- **Airflow DAG** — co-developed the scheduled PDF ingestion and preprocessing pipeline

---

## Project Structure

```
CFA-PDFs-RAG/
├── airflow/          # DAGs for PDF ingestion and embedding pipeline
├── fastapi/          # Backend — /ingest and /query endpoints
├── streamlit/        # Frontend — PDF upload and Q&A interface
└── docker-compose.yml
```

---

## Getting Started

### Prerequisites
- Python 3.9+
- Docker & Docker Compose
- OpenAI API key
- Pinecone API key

### Setup

```bash
git clone https://github.com/shardulchavan/CFA-PDFs-RAG
cd CFA-PDFs-RAG

# Fill in your environment variables
cp .env.example .env

# Start all services
docker-compose up --build
```

Then open:
- Streamlit app: `http://localhost:8501`
- FastAPI docs: `http://localhost:8000/docs`
- Airflow UI: `http://localhost:8080`

---

## How it works

1. Drop a CFA PDF into the app or let Airflow pick it up automatically
2. The pipeline extracts text, chunks it into ~500-token segments, and generates OpenAI embeddings
3. Embeddings are indexed in Pinecone with metadata (page number, section)
4. Ask any question in plain English — e.g. *"What is the difference between systematic and unsystematic risk?"*
5. The system retrieves the 3 most relevant chunks and sends them as context to GPT, which answers with source references

---

## Topics

`rag` `openai` `pinecone` `airflow` `fastapi` `streamlit` `pdf-extraction` `llm` `vector-database` `python`




