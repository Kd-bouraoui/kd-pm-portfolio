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

### CoachRunning Agentic System *(live · private repo)*

Automated running coach for 6 athletes. Generates personalized programs using La Clinique du Coureur methodology and pushes them directly to athletes' TrainingPeaks calendars — which auto-sync to their Suunto/Garmin watches.

**The problem:** Personalizing a week of training for 6 athletes manually takes 4+ hours. Each athlete has a different threshold pace, target race, injury history, and schedule. Delivery via PDF means athletes forget the program by Wednesday.

**The discovery:** Started with a Google Drive pipeline (questionnaire → Claude → Google Doc in athlete's folder). Athletes read it once. Built TrainingPeaks integration after realizing the friction was in delivery, not generation. TP syncs directly to the watch — the workout shows up step-by-step during the run.

**The pipeline:**
```
Questionnaire (Google Doc)
      ↓
read_questionnaire.py  ← extracts profile, goals, constraints via Drive API
      ↓
/running-program-designer (Claude Code skill)
      ↓
workouts-<athlete>.json  ← structured workout definitions per week + "pourquoi" text per session
      ↓
push_to_trainingpeaks.py  ← pushes structured workouts as coach to each athlete's TP calendar
      ↓
TrainingPeaks  ──auto-sync──▶  Suunto Race S / Garmin 965
```

**The hard technical parts:**
- TP's API requires a partnership for OAuth — used cookie-based auth (`Production_tpAuth`) that exchanges for a bearer token. Stored in `~/.tp-coach.json`, expires ~30 days
- Pace targets must be sent as % of threshold pace, not min/km — each athlete has a stored `threshold_pace_sec`, the script converts every range at push time
- Workout structure is a serialized JSON string inside the API payload (not a nested object) — required reverse-engineering the polyline format for the pace graph display
- Multi-athlete coach mode: authenticates as coach, fetches `athlete_id` from `user.athletes[]`, posts workouts to each athlete's calendar

**Eval system — two layers:**

`/test-coaching-fonctionnel` (functional regression, binary assertions):
- Correct zones for each step type (warmup → Z1, intervals → Z4+)
- Allures within bounds for the athlete's threshold
- JSON structure valid for TP push
- Décharge (taper) weeks flagged correctly

`/eval-coaching-qualite` (LLM-as-judge, gradient 0–1 score):
- Conformité aux sources LDCC/DR
- Précision des allures
- Cohérence de la progression semaine sur semaine
- Ton du "Pourquoi cette séance" (no jargon, athlete-first language)
- Structure du bilan
- Prise en compte du feedback athlète

Both run automatically before every program delivery or weekly review. No output ships without passing.

**Metrics:** 6 athletes · 22 weeks programmed · 61 workouts structured and delivered · Weekly automated review cycle

---

### PM Knowledge OS *(live · personal use)*

Agentic system for ingesting PM articles, organizing them into a structured second brain, and retrieving them at the right moment in product work.

**The problem:** Reading a PM article takes 20 minutes. Extracting what's applicable to current work, categorizing it, and making it retrievable later takes another hour. Most PMs bookmark things and never go back.

**What I built:**
- `/strategic-educator` — reads an article (Markdown or PDF) and produces a structured 4-section French synthesis: 30s thesis, key concepts with PM impact, process walkthrough, strategic takeaway
- `/categorisation` — PM librarian: analyzes a Sources/ folder, proposes reading order (fundamentals → technology → PM application), renames files with numeric prefixes after validation
- `/pm-prompt-generator` — transforms raw PM task descriptions into structured production-ready prompts using XML tags, saves them to the correct Prompt Library subfolder
- 7-domain routing table in `CLAUDE.md` — any PM analysis is automatically directed to the relevant source files without re-instruction

**The non-obvious constraint:** Ingestion friction kills knowledge systems. A 30-second synthesis filed immediately beats a perfect note never written. Every skill removes one step of friction from the ingestion loop.

**Architecture:** Local files + Claude Code rather than Notion or Obsidian. Tradeoff: no mobile access, but full schema control and no vendor lock-in. The `CLAUDE.md` is the schema — Claude reads it at session start and respects it across conversations.

**Scale:** 7 PM domains · 50+ synthesized sources · Prompt library across 4 categories

---

### Job Search AI OS *(built · personal use)*

10+ orchestrated Claude Code skills covering offer analysis, CV tailoring, cover letter generation, interview prep, compensation targeting, and daily multi-workspace check-ins.

**The problem:** Senior PM job searching is a full-time job. Each application needs tailored positioning, competitive intel, and prep — most candidates do this manually across 8–12 active processes simultaneously.

**Skills built:** `/matchmaker` (fit scoring on 6 dimensions) · `/job-fit` (track record vs requirements gap) · `/cover-letter-generator` · `/outreach-generator` · `/interview-prep` · `/interview-debrief` · `/compensation-target` · `/daily-briefing` · `/candidate-market-fit` · `/referral-coach` · `/inbound-reply-generator`

**Pipeline:** `pipeline.md` is the single source of truth — every skill reads and writes to it. `/daily-briefing` surfaces what needs action each morning across all active processes.

**The hard design problem:** Defining when the system should stop and ask a human. Every skill has explicit human checkpoints — no email is sent, no pipeline stage updated, no commitment made without a visible confirmation step. That boundary is a product design decision, not a technical one.

---

### doc-immo *(in progress · private)*

AI system that analyzes mortgage document packages and produces a structured synthesis for brokers.

**The problem:** Mortgage brokers spend 30–60 min manually reviewing client document packages before they can start their analysis. Files come in different formats, naming conventions, and completeness levels.

**The approach:** Upload → Claude extracts key fields (income, liabilities, red flags) → structured output with confidence flags for human review. Build-to-learn first: validate extraction reliability before building any UI.

**Tradeoff:** Claude over fine-tuned models — document types vary too much for a narrow model to generalize. Latency is acceptable; this is not a real-time use case.

---

## How I work with AI

I use AI as an accelerant for product work — not a replacement for PM judgment.

- **Prototyping:** V0 for production-ready UI mockups delivered directly to Engineering
- **Research synthesis:** Claude Code for processing user interviews, community feedback, competitive signals
- **Experimentation:** Statsig + CUPED methodology for A/B tests on noisy e-commerce data
- **Systems design:** Agentic workflows with explicit tool boundaries and human validation checkpoints

The output isn't "AI-generated content." It's faster, better-instrumented PM work.

---

*Senior PM based in Montréal · kdbouraoui@gmail.com · [LinkedIn](https://linkedin.com/in/kdbouraoui)*
