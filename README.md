# KD PM Portfolio

**AI-augmented product work** — tools I build and use as a Senior PM.

> Not tutorials. Not demos. Things I actually ship and use daily.

---

## Shipping log

| When | What shipped |
|------|-------------|
| May 2026 | CoachRunning TP integration — multi-athlete coach mode, pace zone conversion per athlete, auto-sync to watches |
| May 2026 | Automated weekly review — 6 athletes, planned vs done comparison, next week generation, coach notes pushed to TP |
| May 2026 | Session debrief pipeline — fetches TP data post-run, inserts lap-by-lap analysis + LDCC commentary into coach MD |
| Apr 2026 | CoachRunning eval system — LLM-as-judge on 6 quality dimensions + functional regression tests, auto-run before every delivery |
| Apr 2026 | CoachRunning Google Drive pipeline — questionnaire → Claude → Google Doc delivered to athlete's Drive |
| Mar 2026 | Job Search AI OS — 10+ orchestrated skills: offer analysis, CV tailoring, outreach, interview prep, daily check-in |
| Feb 2026 | PM Knowledge OS — 7-domain knowledge base with article ingestion pipeline, prompt library, and skill-based retrieval |
| Aug 2025 | doc-immo MVP — Claude extracts mortgage document fields, outputs structured synthesis with confidence flags |

---

## Projects

### CoachRunning *(live · private repo)*

Agentic running coach for 6 athletes. Generates personalized programs using La Clinique du Coureur methodology and pushes them directly to athletes' TrainingPeaks calendars, which auto-sync to their Suunto/Garmin watches.

**AI infra:**
- **Two-layer eval before every delivery** — functional regression (binary assertions: correct zones, valid JSON, pace bounds per athlete) + LLM-as-judge (0–1 score on 6 dimensions: source conformity, pace accuracy, progression coherence, coaching tone, structure, athlete feedback integration). No output ships without passing both.
- **Human-in-the-loop checkpoints** — coach reviews generated program before push; athletes log RPE after each session; weekly debrief surfaces deviations before next week is generated
- **Metrics:** 6 athletes · 22 weeks programmed · 61 workouts delivered · weekly automated review cycle

---

### PM Knowledge OS *(live · personal use)*

Agentic system for ingesting PM articles, organizing them into a structured second brain, and surfacing them at the right moment during product work. 7 PM domains, 50+ synthesized sources, prompt library across 4 categories.

**AI infra:**
- **Routing before generation** — `CLAUDE.md` maps every task type to the right source files; Claude reads them before producing any analysis. No hallucinated frameworks.
- **Friction-first design** — ingestion skill produces a structured synthesis in under 30 seconds; if the path to filing is longer than reading, the system dies. Every skill removes one step.
- **Human gate** — categorization skill proposes reading order and file renaming, waits for explicit approval before touching the file system

---

### Job Search AI OS *(built · personal use)*

10+ orchestrated Claude Code skills covering offer analysis, CV tailoring, cover letter generation, interview prep, compensation targeting, and daily pipeline check-ins across all active processes.

**AI infra:**
- **Single source of truth** — `pipeline.md` is read and written by every skill; no state lives inside a skill. Portable, inspectable, recoverable.
- **Explicit human checkpoints** — no email sent, no pipeline stage updated, no commitment made without a visible confirmation step. The boundary between AI action and human decision is a product choice, not an afterthought.
- **Eval loop** — `/interview-debrief` captures what landed and what didn't after each interview; findings feed back into prep for the next round

---

### doc-immo *(in progress · private)*

AI system that analyzes mortgage document packages and produces a structured synthesis for brokers. Claude extracts key fields, flags inconsistencies, and outputs confidence-scored results for human review.

**AI infra:**
- **Confidence flags on every extraction** — output marks uncertain fields explicitly; human reviewer focuses effort where the model hedges, not across the full document
- **Build-to-learn first** — extraction reliability validated before any UI is built; no premature productization of an unvalidated model behavior

---

## Stack

Claude Code · TrainingPeaks API · Google Drive API · Statsig (CUPED A/B testing) · LLM-as-judge evals · functional regression testing

---

*Senior PM based in Montréal · kdbouraoui@gmail.com · [LinkedIn](https://linkedin.com/in/kdbouraoui)*
