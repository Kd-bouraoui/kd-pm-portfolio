# KD PM Portfolio

**AI-augmented product work** — tools I build and use as a Senior PM.

> Not tutorials. Not demos. Things I actually ship.

---

## Projects

### doc-immo *(in progress · private)*
AI system that analyzes mortgage documents uploaded by real estate clients and produces a structured synthesis of key elements (income, liabilities, red flags).

**The problem:** Mortgage brokers spend 30–60 min manually reviewing client document packages before they can start their analysis. Files come in different formats, naming conventions, and completeness levels.

**The approach:** Upload → Claude extracts key fields → structured output with confidence flags for human review. Build to Learn first: validate that the extraction logic is reliable before building any UI.

**Tradeoffs:** Chose Claude over fine-tuned models because the document types vary too much for a narrow model to generalize. Latency is acceptable for this use case (not real-time).

---

### CoachRunning Agentic System *(live)*
Agentic training plan generator built on La Clinique du Coureur methodology with direct TrainingPeaks sync via MCP.

**The problem:** Generating personalized running programs manually is time-consuming and hard to keep consistent across multiple athletes.

**The approach:** Claude Code + custom skills → structured workout JSON → TrainingPeaks API via Model Context Protocol → auto-sync to athlete watches (Suunto).

**What I learned:** MCP is genuinely useful for bridging local AI workflows to external APIs without building a full integration layer. The friction is in auth token management, not in the protocol itself.

---

### PM Knowledge OS *(live · personal use)*
Agentic system for ingesting, synthesizing, and curating PM knowledge — articles, frameworks, prompts — organized into a structured second brain.

**The problem:** Reading a PM article takes 20 minutes. Extracting what's actually applicable to current work, filing it in the right place, and making it retrievable later takes an hour. Most PMs just bookmark things and never go back.

**The approach:** Claude Code skills that auto-summarize articles into a structured format (flash thesis · key concepts · strategic takeaway), categorize them by PM domain (discovery, strategy, experimentation, AI product), and generate production-ready prompts from raw task descriptions. Persistent memory across sessions so context doesn't reset.

**Tradeoffs:** Built on local files + Claude Code rather than a hosted knowledge base (Notion, Obsidian). Tradeoff: no mobile access, but full control over the schema and zero vendor lock-in. The system's CLAUDE.md acts as the schema — Claude knows the structure and respects it without being re-instructed each session.

**What I learned:** The bottleneck in a knowledge system isn't storage — it's ingestion. A 30-second summary that gets filed immediately beats a perfect note that never gets written.

---

### Job Search AI OS *(built · personal use)*
10+ orchestrated Claude Code skills covering offer analysis, CV tailoring, cover letter generation, interview prep, compensation targeting, and daily multi-workspace check-ins.

**The problem:** Senior PM job searching is a full-time job. Each application needs tailored positioning, competitive intel, and interview prep — most candidates do this manually and inconsistently.

**The approach:** Persistent memory across sessions, contextual skill triggers, autonomous multi-step orchestration. Not a chatbot — a system with defined tool boundaries, human validation checkpoints, and quality criteria.

**What I learned:** The hardest part isn't the AI prompting — it's defining when the system should stop and ask a human. That's a product design problem, not a technical one.

---

## How I work with AI

I use AI as an accelerant for product work — not a replacement for PM judgment.

- **Prototyping:** V0 for production-ready UI mockups (delivered directly to Engineering)
- **Research synthesis:** Claude Code for processing user interviews, community feedback, competitive signals
- **Experimentation:** Statsig + CUPED methodology for A/B tests on noisy e-commerce data
- **Systems design:** Defining agentic workflows with explicit tool boundaries and human checkpoints

The output isn't "AI-generated content." It's faster, better-instrumented PM work.

---

*Senior PM based in Montréal · kdbouraoui@gmail.com · [LinkedIn](https://linkedin.com/in/kdbouraoui)*
