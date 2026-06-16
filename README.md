# Dockerized AI Agent

A RAG-powered AI assistant with a Python Flask web UI, PostgreSQL chat history, and local LLM inference — fully containerized and deployed on AWS EC2.

**Stack:** Python · Flask · LangChain · ChromaDB · PostgreSQL · Docker Compose · Ollama (Llama 3.1 8B) · AWS EC2

---

## Overview

This project is a production-style Retrieval-Augmented Generation (RAG) pipeline that lets users ask natural-language questions over a restaurant review dataset through a browser-based chat interface. Responses are grounded in retrieved documents rather than model hallucination, and full conversation history is persisted across sessions in PostgreSQL.

The stack runs entirely in Docker containers on a single AWS EC2 instance (t3.xlarge) with a live public endpoint.

---

## Features

- **RAG pipeline** — LangChain retrieves relevant chunks from ChromaDB and injects them into the LLM prompt for context-aware, grounded answers
- **Local LLM inference** — Llama 3.1 (8B) runs via Ollama; no external API dependency or per-token costs
- **Persistent chat history** — PostgreSQL stores full conversation state across sessions via `database.py`
- **Browser UI** — Flask serves a chat interface from the `templates/` directory
- **Single-command startup** — `start.sh` brings up the full stack; Docker Compose handles service orchestration, health checks, and networking

---

## Architecture

```
Browser
   │
   ▼
Flask (app.py)
   │
   ├──► ChromaDB (vector.py)         ← semantic document retrieval
   │         │
   │         ▼
   │     Ollama / Llama 3.1 8B       ← local LLM inference
   │
   └──► PostgreSQL (database.py)     ← chat history persistence
```

---

## Project Structure

```
.
├── app.py                             # Flask routes and RAG chain
├── main.py                            # Entry point
├── vector.py                          # ChromaDB ingestion and retrieval
├── database.py                        # PostgreSQL connection and history
├── templates/                         # Flask HTML templates (chat UI)
├── chrom_langchain_db/                # Persisted ChromaDB vector store
├── realistic_restaurant_reviews.csv   # Source dataset
├── docker-compose.yml                 # Service orchestration
├── Dockerfile                         # Flask app container
├── requirements.txt
├── start.sh                           # Startup script
└── .dockerignore
```

## Usage

Navigate to the Flask UI in your browser and start asking questions about the restaurant review dataset:

> *"Which restaurants have the best pasta reviews?"*  
> *"Are there any highly rated vegan spots?"*

The assistant retrieves the most relevant reviews from ChromaDB, passes them as context to Llama 3.1, and returns a grounded answer — with the full conversation saved to PostgreSQL.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Web framework | Flask |
| RAG orchestration | LangChain |
| Vector store | ChromaDB |
| LLM | Llama 3.1 8B via Ollama |
| Database | PostgreSQL |
| Containerization | Docker Compose |
| Cloud | AWS EC2 (t3.xlarge) |
