# 1️⃣4️⃣ Company Preparation

> Part of the [Interview Handbook](README.md). General interview-process patterns per company type. **Always verify current specifics** (round count, take-home presence, level names) via the company's own careers/engineering blog before an interview — processes change frequently and this guide won't stay perfectly current on its own.

## 📑 Contents
- [How to Use This Section](#how-to-use-this-section)
- [Frontier AI Labs (OpenAI, Anthropic, Google DeepMind)](#frontier-ai-labs-openai-anthropic-google-deepmind)
- [Big Tech (Microsoft, Amazon, Meta, Apple, NVIDIA)](#big-tech-microsoft-amazon-meta-apple-nvidia)
- [AI-Native Startups (Scale AI, Together AI, Perplexity)](#ai-native-startups-scale-ai-together-ai-perplexity)
- [Preparation Strategy by Company Type](#preparation-strategy-by-company-type)

---

## How to Use This Section
Every company's process is a variation on the same core rounds: **recruiter screen → technical screen(s) → onsite/virtual-onsite loop (coding, system design, ML/domain-specific, behavioral) → hiring committee/decision → offer.** What differs is emphasis (how much system design vs pure coding vs ML depth), pace, and culture-fit signals. Prepare the fundamentals in the other guides first — this section tells you where to weight your prep time per company type.

## Frontier AI Labs (OpenAI, Anthropic, Google DeepMind)

**Interview process**: typically a recruiter screen, one or two technical/coding screens, a take-home or pairing exercise for research/engineering-adjacent roles, then an onsite loop mixing coding, ML/systems depth, and a strong behavioral/values-fit component. Research roles add a research discussion round (walk through your past work in depth).

**What's commonly emphasized**:
- Strong fundamentals over trivia — expect to be asked to reason through a novel problem rather than recite a memorized answer.
- For applied/engineering roles: production ML systems experience (serving, evaluation, data pipelines) is often weighted as heavily as modeling knowledge.
- For all roles: genuine engagement with the company's mission and safety/alignment philosophy tends to matter more here than at typical big tech — read their published research/blog posts, don't just skim the careers page.
- Coding rounds tend to value clean, well-reasoned code and clear communication over speed-solving obscure algorithms.

**Behavioral**: expect deep "why this company, why now" questions — generic answers are noticeable. Have specific, honest reasons tied to their actual published work or product.

**Preparation strategy**: read recent technical blog posts/papers from the specific lab, be ready to discuss a project of yours in real depth (not just what you built, but why you made each design decision), and practice explaining trade-offs out loud, not just producing correct answers.

## Big Tech (Microsoft, Amazon, Meta, Apple, NVIDIA)

**Interview process**: recruiter screen → 1-2 technical phone screens (coding) → onsite/virtual loop of 4-6 rounds mixing coding, system design, and behavioral, often ending with a hiring committee review rather than the interviewers deciding directly.

**Company-specific patterns**:
- **Amazon**: heavy emphasis on behavioral questions mapped to their Leadership Principles — prepare 2-3 STAR stories per principle (Ownership, Bias for Action, Customer Obsession, etc.), because nearly every round includes at least one behavioral question.
- **Meta**: strong emphasis on coding speed/correctness (often timed, multiple problems per round) plus a "Jedi"/culture-fit round; system design rounds expect familiarity with Meta-scale problems (news feed, messaging at billions of users).
- **Microsoft**: broader mix, less rigid than Amazon's LP framework, often includes a round specifically with the hiring manager assessing collaboration style.
- **Apple**: team-specific loops vary widely; expect deep technical questions tied directly to the specific team's domain (not generic algorithms).
- **NVIDIA**: for ML/AI infra roles, expect strong systems + CUDA/parallel computing depth if the role touches performance-critical code; less emphasis on generic LeetCode-style rounds for specialized infra teams.

**Preparation strategy**: know the company's stated values/leadership principles cold (especially Amazon), practice timed coding under interview conditions, and prepare a system design answer at the scale that specific company actually operates at.

## AI-Native Startups (Scale AI, Together AI, Perplexity)

**Interview process**: typically faster and leaner than big tech — fewer rounds, often a practical take-home or pairing session that closely mirrors real day-to-day work, plus a founder/exec round assessing conviction and pace-of-execution fit.

**What's commonly emphasized**:
- Hands-on, current knowledge of the LLM/AI stack (the [LLM Interview guide](06_llm_interview.md) and [AI Agents guide](07_ai_agents.md) are directly relevant) — startups expect you to already speak the domain language fluently, less hand-holding than a big-company screen.
- Ownership and speed: be ready to discuss times you shipped something end-to-end with ambiguous requirements and limited support.
- Startup-stage awareness: know roughly what stage/size the company is at and be able to articulate why that specific stage appeals to you versus a larger, safer employer.

**Preparation strategy**: build something small and real with their actual product/API before the interview if possible, be ready to talk numbers (latency, cost, scale) from real projects, and expect the behavioral round to probe risk tolerance and autonomy more than tenure/process fit.

## Preparation Strategy by Company Type

| Company type | Weight coding prep | Weight system design | Weight behavioral | Weight domain/ML depth |
|---|---|---|---|---|
| Frontier AI labs | Medium | Medium-High | High (mission fit) | Very High |
| Big tech | High | High | High (esp. Amazon) | Medium |
| AI-native startups | Medium | Medium | Medium (ownership focus) | Very High |

**General rule of thumb**: 2-3 weeks out, split practice time roughly 40% coding, 30% system design, 20% behavioral story prep, 10% company-specific research — then shift more toward company-specific research and mock interviews in the final week.

---
*Part of the [AI Engineer Handbook](../../README.md) · [Interview Handbook](README.md) · Next: [Hiring Preparation](15_hiring_preparation.md).*
