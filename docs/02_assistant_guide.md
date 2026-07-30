# 2️⃣ AI Assistant Guide

## What I am
I am an **AI assistant** – a large language model (LLM) that can understand natural language, reason, and generate text, code, or structured data.

## How I work
1. **Tokenisation** – your prompt is split into tokens.
2. **Context window** – the model attends to up to *N* tokens (e.g., 8 k).
3. **Transformer layers** – self‑attention mixes information across all tokens.
4. **Decoding** – the model predicts the next token repeatedly until a stop condition.
5. **Tool usage** – via **MCP** the model can request external functions (search, code execution, etc.).

## Strengths
- Fast knowledge synthesis from billions of parameters.
- Strong code generation and reasoning when given proper prompts.
- Ability to call tools, browse, and maintain state across calls.

## Limitations
- **Hallucinations** – may generate plausible‑looking but incorrect facts.
- **Context length** – cannot see data beyond the window.
- **No real‑time perception** – relies on static inputs unless tools provide fresh data.

## Hallucinations
When the model guesses information it does not actually know, it is called a hallucination. Mitigation strategies:
- Use **retrieval‑augmented generation** (RAG).
- Verify facts against trusted sources.
- Keep prompts concise and ask for citations.

## Memory
- **Short‑term**: conversation history stored in the context window.
- **Long‑term**: external vector stores, databases, or files that the model can query via functions.

## Context Management
- Summarise older turns.
- Use **function calls** to fetch relevant data instead of stuffing everything into the prompt.

## Tool Usage Best Practices
| Situation | Recommended tool |
|-----------|------------------|
| Look up a recent news article | `web_search` |
| Execute a shell command | `terminal` |
| Read a large file | `read_file` |
| Perform a complex calculation | `execute_code` |
| Store persistent data | `memory` |

---

*This guide is deliberately concise; each bullet can be expanded into a full chapter in the handbook.*