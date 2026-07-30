# 9️⃣ Examples

This folder contains **complete, runnable examples** for every major concept covered in the handbook.  Each example is a self‑contained mini‑project that can be cloned, executed, and inspected.

## 9.1 Chatbot with RAG (`chatbot_rag/`)
- `app.py` – FastAPI service implementing the RAG workflow (see 7.1).
- `Dockerfile` – containerises the service.
- `requirements.txt` – Python dependencies.
- `run.sh` – helper script to start locally with Docker Compose.

## 9.2 Autonomous Agent (`agent/`)
- `agent.py` – minimal autonomous agent loop (see 7.2).
- `tools.py` – wrappers for web search, code execution, file I/O.
- `Dockerfile` – builds a lightweight image.

## 9.3 Coding Assistant (`coding_assistant/`)
- WebSocket server that streams code diffs (see 7.3).
- Example React client that displays suggestions live.

## 9.4 Resume Analyzer (`resume_analyzer/`)
- FastAPI endpoint that accepts a PDF, runs OCR, classifies sections, and returns JSON feedback (see 7.4).

## 9.5 Enterprise RAG (`enterprise_rag/`)
- Full‑stack repo with Pinecone vector store, FastAPI backend, and React frontend for searching a corporate knowledge base (see 7.5).

## 9.6 SaaS MVP (`ai_saas_mvp/`)
- End‑to‑end monorepo containing:
  - `frontend/` – React + Vite UI.
  - `backend/` – FastAPI service with JWT auth, RAG, PostgreSQL.
  - `helm/` – Helm chart for Kubernetes deployment.
  - `docker-compose.yml` – local dev stack.

## 9.7 Hackathon Idea Generator (`hackathon_idea/`)
- LangChain agent that builds a product spec, mock UI, and creates a starter GitHub repo (see 7.7).

## 9.8 Startup MVP Blueprint (`startup_mvp/`)
- Step‑by‑step guide with scripts to bootstrap an AI‑first startup, including Stripe integration, feature flags, and CI/CD.

> **All examples are deliberately lightweight** – they use free‑tier services where possible (e.g., Qdrant Cloud, OpenAI trial).  Replace the placeholder API keys with your own before running.
