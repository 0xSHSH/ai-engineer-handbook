# 1️⃣ AI Fundamentals

## What is AI?
Artificial Intelligence (AI) is the field of computer science that aims to create systems capable of performing tasks that normally require human intelligence – reasoning, perception, learning, and decision‑making.

## Large Language Models (LLMs)
LLMs are neural networks trained on massive text corpora. They predict the next token given a context and can generate coherent prose, code, or reasoning steps.

## Transformers
The transformer architecture introduced **self‑attention**, enabling models to weigh every token in the input when producing each output token. This replaced recurrent networks and made scaling to billions of parameters feasible.

## Tokens
A token is the smallest unit the model processes (often a sub‑word piece). Tokenization converts raw text into a sequence of IDs that the model consumes.

## Embeddings
Tokens are mapped to high‑dimensional vectors (embeddings) that capture semantic meaning. These vectors are the model’s input to the transformer layers.

## Vector Databases
Specialized databases (e.g., Pinecone, Qdrant, Milvus) store embeddings and provide fast **similarity search** (approximate nearest neighbor) for Retrieval‑Augmented Generation (RAG).

## Retrieval‑Augmented Generation (RAG)
RAG combines a **retriever** (vector DB) with a **generator** (LLM). The retriever fetches relevant passages, and the generator conditions on them to produce grounded answers.

## Fine‑tuning
Fine‑tuning updates a pre‑trained model on a domain‑specific dataset, improving performance on niche tasks while retaining general capabilities.

## AI Agents
Agents are LLMs equipped with **tool use** (function calling, code execution, browsing) and a **controller** that decides which tool to invoke to achieve a goal.

## Model‑Calling‑Protocol (MCP)
MCP is a structured JSON schema that defines how a model requests external tool execution and receives results, enabling safe, deterministic interactions.

## Function Calling
Function calling lets the model output a JSON payload describing a function name and arguments; the system then executes the function and feeds the result back to the model.

## Context Windows
The context window is the maximum number of tokens the model can attend to at once (e.g., 4 k, 8 k, 32 k). Anything beyond is truncated or needs a sliding‑window strategy.

## Memory
Memory refers to **persistent state** across calls – short‑term (conversation history) and long‑term (knowledge bases, vector stores). Proper memory management mitigates hallucinations and improves consistency.
