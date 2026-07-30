# 5️⃣ Prompt Engineering Handbook

## 5.1 Beginner Prompts
- Keep it short and explicit.
- Example: `"Summarize the following article in three sentences:"`.

## 5.2 Intermediate Prompts
- Add context, role, and desired format.
- Example: `"You are a senior data scientist. Explain the difference between L1 and L2 regularisation, and provide a Python code snippet demonstrating both."`

## 5.3 Expert Prompts
- Chain‑of‑thought, few‑shot, and structured output.
- Example: `"Given the table below, infer the missing values using linear regression. Output a JSON object with the filled table."`

## 5.4 Chain‑of‑Thought Guidance
- Ask the model to *think step by step* before answering.
- Prompt pattern: `"Let's think step by step. ..."`

## 5.5 Structured Prompts
- Use Markdown tables, XML, or JSON to enforce structure.
- Example JSON prompt:
```json
{
  "task": "extract_entities",
  "text": "...",
  "output_schema": {"Person": [], "Organization": []}
}
```

## 5.6 XML Prompts
```xml
<prompt>
  <instruction>Translate the following English sentence to French.</instruction>
  <input>Hello, world!</input>
</prompt>
```

## 5.7 Markdown Prompts
```markdown
**Instruction:** Write a concise README for a Python CLI tool.
**Constraints:** Use only standard library, no external dependencies.
```

## 5.8 System Prompts
- Set the *persona* and *behaviour* of the assistant.
- Example: `"You are a helpful AI coding assistant that never fabricates code without tests."`

## 5.9 Role Prompts
- Define a role for the model to adopt.
- Example: `"You are a senior DevOps engineer reviewing a Kubernetes manifest."`

## 5.10 Few‑Shot Prompting
- Provide a few examples of input–output pairs.
- Useful for classification, transformation, or pattern extraction.

## 5.11 Common Mistakes
| Mistake | Why it hurts | Fix |
|---------|--------------|-----|
| Over‑loading the prompt with unrelated data | Dilutes focus, exceeds context window | Keep only relevant information |
| Assuming the model will *always* follow instructions | Hallucinations happen | Ask for citations or verification |
| Not specifying output format | Model may return free‑form text | Explicitly request JSON/Markdown |
| Ignoring temperature settings | May get non‑deterministic answers | Set `temperature=0` for reproducibility |

## 5.12 Prompt Optimization Checklist
1. Define goal clearly.
2. Choose appropriate role/system prompt.
3. Add constraints (format, length, style).
4. Provide examples if needed.
5. Ask for verification or sources.
6. Test with variations and iterate.

---

*All sections can be expanded with domain‑specific examples (e.g., data‑science, security, robotics).*