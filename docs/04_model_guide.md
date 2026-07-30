# 4️⃣ Model Guide

## Overview
Choosing the right model is a **trade‑off** between latency, cost, reasoning depth, and available capabilities (code, vision, multimodal, long‑context). This guide categorises the most common model families and provides a decision matrix.

---

## 4.1 Model Families
| Family | Typical Use‑case | Context window | Parameter count | Strengths | Weaknesses |
|--------|------------------|----------------|----------------|----------|------------|
| **Fast (Chat) models** (e.g., GPT‑3.5‑turbo, Claude‑instant) | Real‑time chat, simple code snippets, classification | 4k‑8k | 6‑13B | Low latency, cheap | Limited reasoning depth, struggles with complex math |
| **Reasoning models** (e.g., GPT‑4‑turbo, Claude‑2) | Complex planning, multi‑step reasoning, chain‑of‑thought | 8k‑32k | 100B+ | Strong logical abilities, better factuality | Higher cost, slower |
| **Coding models** (e.g., CodeLlama‑34B, Gemini‑code) | Code generation, bug fixing, refactoring | 8k‑16k | 34B‑70B | Optimised for syntax, can emit runnable code | May hallucinate APIs, limited natural‑language knowledge |
| **Vision models** (e.g., Gemini‑1.5‑pro‑vision, LLaVA) | Image captioning, OCR, visual QA | 2k‑4k (image tokens) | 30B‑80B | Understands pixels, multimodal prompts | Requires image input handling, higher compute |
| **Multimodal models** (e.g., GPT‑4‑vision, Claude‑3‑sonnet) | Text + image + audio in a single prompt | 8k‑128k | 100B+ | Unified interface, can reason across modalities | Expensive, may need special API endpoints |
| **Long‑context models** (e.g., Claude‑3‑opus‑128k, Llama‑3‑70B‑8192) | Document summarisation, RAG over large corpora | 32k‑128k | 70B+ | Process whole books, contracts without chunking | Memory‑heavy, slower per token |

---

## 4.2 Cost vs. Performance
| Tier | Approx. $/1k tokens (USD) | Latency (ms) per 1k tokens | When to use |
|------|--------------------------|---------------------------|-------------|
| **Free / trial** | $0 | <50 | Prototyping, demos, low‑risk tasks |
| **Cheap (fast)** | $0.0005‑0.001 | 30‑80 | Customer‑support chat, simple CRUD bots |
| **Balanced** | $0.002‑0.003 | 80‑150 | Reasoning, planning, moderate‑size RAG |
| **Premium (reasoning)** | $0.015‑0.03 | 200‑500 | Complex legal analysis, multi‑step code review |
| **Vision / Multimodal** | $0.02‑0.05 | 300‑800 | Image‑based QA, document OCR, UI generation |

---

## 4.3 Decision Matrix
```mermaid
flowchart TD
    A[What is the primary modality?] -->|Text only| B[Need fast response?]
    A -->|Image or audio| C[Pick Vision/Multimodal model]
    B -->|Yes| D[Fast chat model (e.g., gpt‑3.5‑turbo)]
    B -->|No| E[Reasoning model (e.g., gpt‑4‑turbo)]
    D --> F[Is code generation required?]
    E --> F
    F -->|Yes| G[Coding‑optimised model]
    F -->|No| H[General LLM]
```

---

## 4.4 Practical Tips
1. **Start cheap** – prototype with the cheapest model that meets latency requirements; upgrade only if quality suffers.
2. **Benchmark** – run a few representative prompts on candidate models and measure `accuracy` (e.g., exact match) and `latency`.
3. **Hybrid pipelines** – use a fast model for routing and a reasoning model for the heavy‑lift step.
4. **Cache** – for RAG, cache embeddings and even LLM responses for identical queries to save cost.
5. **Safety** – always enforce a content‑filter layer regardless of model size.

---

## 4.5 Model‑Specific Caveats
| Model | Known quirks |
|-------|-------------|
| **GPT‑3.5‑turbo** | May truncate long code blocks; avoid >4k tokens.
| **Claude‑instant** | Strong at classification but limited token window (4k).
| **CodeLlama** | Requires proper `language` tag in prompts; otherwise may produce generic text.
| **Gemini‑vision** | Image size limit 5 MB; use base64 encoding.
| **Llama‑3‑70B‑8192** | Needs quantisation for cheap inference; watch for OOM on CPU.

---

*The guide is deliberately model‑agnostic; replace the placeholder model names with the ones you have access to.*