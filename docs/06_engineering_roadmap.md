# 6️⃣ Engineering Roadmap

## Overview
The **AI Engineering Roadmap** is a progressive learning path that takes a developer from a brand‑new Python user to a production‑ready AI engineer capable of designing, building, and operating large‑scale AI systems.  Each milestone lists the core concepts, recommended resources, and a concrete project that validates the skill.

---

## 📚 Phase 0 – Foundations (Weeks 1‑2)
| Skill | What to learn | Resources | Mini‑project |
|-------|---------------|-----------|--------------|
| **Python basics** | Variables, control flow, functions, data structures | *Automate the Boring Stuff* (Ch. 1‑6) – free online; Python docs | CLI tool that parses a CSV and prints summary statistics |
| **Git & GitHub** | Commits, branches, PR workflow, rebasing | *Pro Git* (Ch. 1‑3) – free online | Create a repo, push the CSV parser, open a PR with a small improvement |
| **Linux CLI** | Navigation, piping, `grep`, `awk`, `sed`, `chmod` | LinuxCommand.org – quick reference | Write a Bash script that backs up the CSV daily |

---

## 📦 Phase 1 – Core Development (Weeks 3‑6)
| Skill | What to learn | Resources | Mini‑project |
|-------|---------------|-----------|--------------|
| **Virtual environments & packaging** | `venv`, `pip`, `pyproject.toml` | Real‑Python – *Packaging Python Projects* | Turn the CSV parser into a pip‑installable package |
| **APIs with FastAPI** | Async endpoints, OpenAPI, validation, testing | *FastAPI Documentation* – official tutorial | Build a REST endpoint that returns the CSV summary as JSON |
| **Docker** | Container basics, `Dockerfile`, multi‑stage builds | Docker Docs – *Get Started* | Containerise the FastAPI service |
| **Testing** | `pytest`, fixtures, coverage, CI | *Python Testing with pytest* (book) | Write unit tests for the parser and FastAPI routes |
| **CI/CD** | GitHub Actions, linting, automated tests, Docker push | GitHub Actions docs – *Python package workflow* | Add a workflow that runs tests on every PR |

---

## ☁️ Phase 2 – AI‑Specific Skills (Weeks 7‑12)
| Skill | What to learn | Resources | Mini‑project |
|-------|---------------|-----------|--------------|
| **NumPy & pandas** | Data manipulation, vectorised ops | *Python Data Science Handbook* (Ch. 2‑4) | Load a larger dataset, compute statistics, visualise with `matplotlib` |
| **Scikit‑learn** | Classic ML pipelines, model evaluation | *Introduction to ML with Python* (O'Reilly) | Build a simple regression model for house‑price prediction |
| **LLM APIs** | OpenAI, Anthropic, Gemini, Azure OpenAI | Official API docs (quickstart) | Write a wrapper that sends a prompt and streams the response |
| **Prompt Engineering** | Prompt patterns, few‑shot, chain‑of‑thought | This handbook’s Prompt Engineering chapter | Create a prompt library for classification, summarisation, code generation |
| **Vector Databases** | Pinecone, Qdrant, Chroma | Official tutorials | Store embeddings of a document corpus and implement a RAG endpoint |
| **LangChain / LangGraph** | Chains, agents, memory, callbacks | LangChain docs – *Getting Started* | Build a simple agent that can answer FAQs using the RAG store |
| **MLOps basics** | Model versioning, CI for models, monitoring | *Continuous Delivery for Machine Learning* (O'Reilly) | Add a GitHub Action that validates a model’s inference on a test set |

---

## 🚀 Phase 3 – Production & Scaling (Weeks 13‑20)
| Skill | What to learn | Resources | Mini‑project |
|-------|---------------|-----------|--------------|
| **Kubernetes** | Pods, Deployments, Services, ConfigMaps, Secrets, Helm | *Kubernetes Up & Running* (2nd ed.) | Deploy the FastAPI + RAG service to a k8s cluster (kind or GKE) |
| **Observability** | Logs (ELK), Metrics (Prometheus/Grafana), Traces (Jaeger) | CNCF Observability Landscape | Instrument the service with OpenTelemetry and view traces |
| **Security** | Secrets management, IAM, scanning containers, rate‑limiting LLM calls | OWASP API Security Top 10 | Add JWT auth to FastAPI, scan Docker images with Trivy |
| **Cost optimisation** | Token budgeting, caching, quantisation | Model provider pricing docs | Implement a cache layer for RAG queries and measure cost reduction |
| **A/B testing & rollout** | Canary deployments, feature flags | *Feature Flags at Scale* (article) | Deploy two model versions behind a flag and collect user feedback |
| **Compliance** | GDPR, data retention, model audit logs | EU AI Act summary (2024) | Add audit logging for all user queries |

---

## 📈 Phase 4 – Advanced Topics (Weeks 21‑∞)
| Skill | What to learn | Resources | Project |
|-------|---------------|-----------|--------|
| **Fine‑tuning** | LoRA, QLoRA, parameter‑efficient tuning | HuggingFace `trl` docs | Fine‑tune a 7B model on a domain‑specific dataset |
| **Multimodal AI** | Vision‑LLM pipelines, audio transcription | LLaVA, Whisper docs | Build an image‑question answering bot |
| **Long‑context RAG** | Retrieval over 100k+ documents, chunking strategies | LangChain Long‑Context guide | Deploy a legal‑document search system |
| **Distributed training** | DeepSpeed, ZeRO, FSDP | DeepSpeed docs | Train a 13B model on 4 GPUs |
| **AI safety & alignment** | Red‑team testing, interpretability | *Concrete Problems in AI Safety* (paper) | Implement a safety filter using a secondary LLM |
| **Productisation** | SaaS billing, multi‑tenant architecture, SLAs | *Building AI Products* (course) | Turn the chatbot into a paid SaaS offering |

---

## 📌 How to Use This Roadmap
1. **Pick a phase** that matches your current skill level.
2. **Complete each mini‑project** before moving to the next skill.
3. **Track progress** in a personal repo – create a branch per phase and open a PR when you finish.
4. **Iterate** – the roadmap is not linear; you can revisit earlier phases for deeper mastery.

---

*The roadmap is intentionally modular; you can drop in or skip topics based on your career goals.*