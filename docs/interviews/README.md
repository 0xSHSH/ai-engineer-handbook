# 🎤 AI Engineer Interview Handbook

This folder is a standalone track inside the handbook: everything you need to go from *"I know how to build AI systems"* to *"I got the offer."*

It's split into two layers, because they test completely different things:

| Layer | What it tests | What fails candidates who skip it |
|---|---|---|
| **Getting the interview** | Resume, GitHub, LinkedIn, portfolio | Strong engineers get zero callbacks because a recruiter (or an ATS) never saw them as a match |
| **Passing the interview** | HR/behavioral, technical, system design, coding | Strong builders freeze under structured questioning because they've never practiced *narrating* their thinking |

## 📂 Contents

| # | Document | Covers |
|---|---|---|
| 1 | [`01_resume_preparation.md`](01_resume_preparation.md) | ATS mechanics, resume mistakes, bullet rewriting, GitHub/portfolio/LinkedIn optimization, project selection & presentation |
| 2 | [`02_hr_behavioral.md`](02_hr_behavioral.md) | HR round mechanics, STAR method, common questions with model answers, leadership/ownership/conflict/failure narratives |
| 3 | [`03_technical_python.md`](03_technical_python.md) | Python fundamentals, OOP/SOLID, decorators/generators/context managers, concurrency, memory management, design patterns, error handling |
| 4 | [`04_ml_interview.md`](04_ml_interview.md) | Classical ML algorithms, ensembles, feature engineering, evaluation metrics, bias/variance, hyperparameter tuning |
| 5 | [`05_dl_interview.md`](05_dl_interview.md) | CNN/RNN/LSTM/GRU, transformers & attention, embeddings, transfer learning, optimization, PyTorch vs TensorFlow |
| 6 | [`06_llm_interview.md`](06_llm_interview.md) | Model landscape, tokenization, prompt engineering, RAG, chunking, fine-tuning/LoRA/QLoRA, inference, evaluation, hallucination |
| 7 | [`07_ai_agents.md`](07_ai_agents.md) | Agent architectures, planning, memory, tool/function calling, MCP, multi-agent frameworks, A2A, workflow design |
| 8 | [`08_system_design.md`](08_system_design.md) | Building blocks + worked designs: chat product, RAG system, coding assistant, WhatsApp, Uber, YouTube/Netflix, Gmail |
| 9 | [`09_coding_interview.md`](09_coding_interview.md) | DSA roadmap, complexity cheat sheet, 10 core patterns with code, Blind-75-style practice set by pattern |
| 10 | [`10_sql_guide.md`](10_sql_guide.md) | Joins, indexes, normalization, transactions/ACID, locks, window functions, recursive queries, optimization |
| 11 | [`11_api_guide.md`](11_api_guide.md) | REST, GraphQL, WebSockets, JWT/OAuth, rate limiting, pagination, caching, versioning |
| 12 | [`12_cloud_guide.md`](12_cloud_guide.md) | AWS/Azure/GCP service map, storage, IAM, networking, compute, containers, monitoring |
| 13 | [`13_devops_guide.md`](13_devops_guide.md) | Docker, Kubernetes, CI/CD, GitHub Actions, Terraform, monitoring/logging, deployment strategies |
| 14 | [`14_company_preparation.md`](14_company_preparation.md) | Interview-process patterns for frontier AI labs, big tech, and AI-native startups |
| 15 | [`15_hiring_preparation.md`](15_hiring_preparation.md) | Portfolio/LinkedIn/GitHub checklists, networking, cold-outreach templates, salary negotiation, offer comparison |

> Contributions welcome via the templates in [`08_templates/`](../08_templates/).

## 🧭 How to use this track

```mermaid
flowchart LR
    A[Resume + GitHub + LinkedIn] -->|Get past ATS & recruiter screen| B[Recruiter / HR Screen]
    B -->|Pass behavioral fit check| C[Technical Rounds]
    C --> D[System Design]
    D --> E[Offer]

    style A fill:#1f6feb,color:#fff
    style B fill:#8957e5,color:#fff
```

1. **Start with your resume and profile** ([`01`](01_resume_preparation.md), [`15`](15_hiring_preparation.md)), even if you think it's fine. Most rejections happen before a human ever reads your code — an ATS or a 7-second recruiter scan is often the entire "interview" for that stage.
2. **Rebuild 2-3 behavioral stories** using STAR ([`02`](02_hr_behavioral.md)) before you touch LeetCode. Technical skill gets you shortlisted; behavioral answers get you the offer when two candidates are equally strong.
3. **Drill the technical core**: Python/CS fundamentals ([`03`](03_technical_python.md)), DSA patterns ([`09`](09_coding_interview.md)), and SQL/API/Cloud/DevOps ([`10`](10_sql_guide.md)–[`13`](13_devops_guide.md)) if the role touches infrastructure.
4. **Layer on AI-specific depth**: ML ([`04`](04_ml_interview.md)), DL ([`05`](05_dl_interview.md)), LLMs ([`06`](06_llm_interview.md)), and Agents ([`07`](07_ai_agents.md)) — this is where AI-engineer interviews diverge most from generic SWE interviews.
5. **Practice system design** ([`08`](08_system_design.md)) with worked examples, then read the [company-specific guide](14_company_preparation.md) for your target companies in the final 1-2 weeks.
6. Treat every section here as a **working document** — fill in your own examples inline, don't just read passively.

---
*Part of the [AI Engineer Handbook](../../README.md).*
