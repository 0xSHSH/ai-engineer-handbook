# 7️⃣ AI Agents

> Part of the [Interview Handbook](README.md). Agent architecture, tool/function calling, MCP, multi-agent frameworks, and the questions that test whether you've actually shipped an agent vs read about one.

## 📑 Contents
- [What Makes Something an "Agent"](#what-makes-something-an-agent)
- [Agent Architectures](#agent-architectures)
- [Planning & Reasoning](#planning--reasoning)
- [Memory](#memory)
- [Tool Calling / Function Calling](#tool-calling--function-calling)
- [MCP (Model Context Protocol)](#mcp-model-context-protocol)
- [Multi-Agent Systems](#multi-agent-systems)
- [Framework Comparison](#framework-comparison)
- [Agent Communication (A2A)](#agent-communication-a2a)
- [Workflow Design](#workflow-design)
- [Interview Questions](#interview-questions)

---

## What Makes Something an "Agent"
An LLM call becomes an "agent" when it can: (1) observe an environment/state, (2) decide on an action (including calling tools), (3) execute that action, and (4) loop — using the result to decide the next action — until a goal is met or a stop condition triggers. A single prompt → single response is not an agent; a prompt → tool call → observe result → next decision loop is.

## Agent Architectures

| Pattern | How it works | Good for |
|---|---|---|
| **ReAct** (Reason + Act) | Interleave reasoning traces with tool calls: "Thought → Action → Observation" loop | General-purpose tool-using agents |
| **Plan-and-Execute** | Generate a full multi-step plan upfront, then execute steps (optionally replanning on failure) | Longer tasks where upfront decomposition is cheaper than per-step reasoning |
| **Reflexion / Self-Critique** | Agent critiques its own output and retries before returning | Tasks where correctness matters more than latency (code generation, complex writing) |
| **Finite-state / graph-based** | Explicit state machine defines allowed transitions between steps | Predictable, auditable workflows (customer support triage, approval flows) |

## Planning & Reasoning
- **Task decomposition**: break a large goal into subtasks the model can execute reliably one at a time — reduces compounding error vs asking for the whole solution in one shot.
- **Chain-of-thought vs explicit planning**: CoT reasons inline before acting; explicit planning produces a structured plan (list of steps) that can be inspected, edited, and re-executed independently.
- **Replanning**: after a failed or unexpected tool result, a good agent revises its plan rather than blindly continuing — this is where most naive agent implementations break down.

## Memory

| Type | Scope | Example |
|---|---|---|
| **Short-term / working memory** | Within a single session, held in the context window | Conversation history, current task state |
| **Long-term memory** | Persists across sessions | User preferences, past decisions, learned facts — usually stored externally (vector DB, key-value store, structured DB) and retrieved into context when relevant |
| **Episodic memory** | Records of specific past interactions/events | "Last time this error occurred, this fix worked" |
| **Procedural memory** | Learned skills/workflows | Reusable tool-call sequences, cached successful plans |

Design question interviewers ask: *how do you decide what goes into long-term memory vs gets discarded?* Good answer: explicit summarization/extraction steps, relevance scoring, and human-in-the-loop or confidence thresholds before persisting — not "save everything."

## Tool Calling / Function Calling
The model is given a schema (name, description, parameters) for each available tool. Given a user request, the model outputs a structured call (function name + arguments) instead of, or alongside, natural language. The calling application executes the real function and returns the result to the model as an observation.

```json
{
  "name": "get_weather",
  "description": "Get current weather for a location",
  "parameters": {
    "type": "object",
    "properties": {
      "location": {"type": "string"},
      "unit": {"type": "string", "enum": ["celsius", "fahrenheit"]}
    },
    "required": ["location"]
  }
}
```
**Key design points**: tool descriptions should be precise enough that the model doesn't need to guess; error results should be returned to the model as structured feedback so it can retry/adjust rather than silently failing; and destructive tools (send email, delete data, spend money) should require explicit confirmation rather than being auto-executed.

## MCP (Model Context Protocol)
MCP is an open protocol that standardizes how AI applications connect to external tools, data sources, and systems — instead of every app building bespoke integrations for every tool, both sides implement MCP once. An **MCP server** exposes tools/resources/prompts; an **MCP client** (the AI application) discovers and calls them over a standard interface (stdio or HTTP/SSE transport). This decouples "which tools exist" from "which model/app is using them" — the same MCP server (e.g., a GitHub integration) can be plugged into any MCP-compatible client.

## Multi-Agent Systems
Splitting a task across multiple specialized agents (e.g., a planner, a coder, a reviewer) instead of one generalist agent doing everything.

| Framework | Approach |
|---|---|
| **CrewAI** | Role-based agents ("researcher", "writer") collaborating on a defined process (sequential or hierarchical) |
| **LangGraph** | Graph/state-machine model — agents and tools are nodes, edges define control flow, supports cycles and conditional branching explicitly |
| **AutoGen** | Conversational multi-agent framework — agents "chat" with each other and with humans-in-the-loop to solve a task |
| **OpenAI Agents SDK** | Lightweight primitives for agent loops, handoffs between agents, and guardrails, built around OpenAI's function-calling |
| **Google ADK** | Framework for building, evaluating, and deploying agents with native multi-agent orchestration on Google's stack |
| **Claude Tools / Agent SDK** | Anthropic's tool-use API plus higher-level agent scaffolding (e.g., Claude Code) for coding and computer-use agents |

**Why multi-agent over one big agent**: specialization improves reliability on complex tasks and keeps individual context windows focused, but adds coordination overhead, latency, and new failure modes (agents disagreeing, redundant work, error propagation between agents).

## Agent Communication (A2A)
**Agent-to-Agent (A2A)** protocols standardize how independent agents (potentially built by different vendors/teams) discover each other's capabilities and exchange tasks/results — analogous to what MCP does for agent-to-tool communication, but for agent-to-agent. Key concerns: capability discovery, task delegation format, and trust/authentication between agents that don't share an implementation.

## Workflow Design
Practical checklist when designing an agent workflow:
1. **Define the stop condition** explicitly — otherwise agents loop indefinitely or stop too early.
2. **Bound the tool surface** — give the agent only the tools it needs for this task; a smaller, well-described toolset outperforms a huge generic one.
3. **Add guardrails** for irreversible actions (confirmation steps, dry-run modes, rate limits).
4. **Log every step** (thought, action, observation) for debuggability — agent failures are hard to diagnose without a full trace.
5. **Set a max-iteration / max-cost budget** so a stuck agent fails safely instead of looping forever and burning tokens.

---

## Interview Questions
1. Design an agent that can answer questions about a codebase and open PRs to fix bugs. What tools does it need, and what guardrails?
2. What's the difference between ReAct and Plan-and-Execute, and when would you choose one over the other?
3. How would you prevent an agent from getting stuck in an infinite tool-calling loop?
4. Explain MCP in one paragraph to a non-technical stakeholder.
5. When does a multi-agent system actually outperform a single well-prompted agent? When does it just add overhead?
6. How would you evaluate an agent's performance beyond "did it get the right final answer"? (Step efficiency, tool-call correctness, cost, latency, recoverability from errors.)

---
*Part of the [AI Engineer Handbook](../../README.md) · [Interview Handbook](README.md) · Next: [System Design](08_system_design.md).*
