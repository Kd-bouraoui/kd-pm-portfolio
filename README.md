# KD PM Portfolio

**AI-augmented product work** — tools I build and use as a Senior PM.

> Not tutorials. Not demos. Things I actually ship and use daily.

---

## Shipping log

| When | What shipped |
|------|-------------|
| Oct 2026 | Grounded all nutrition advice (in-run fueling, race-week carb loading, hydration) in a registered dietitians' guide; generated athlete messages can no longer emit figures outside it, enforced by the quality evaluator |
| Oct 2026 | Rebuilt the coaching QA suite from 2,366 lines of prompt-embedded test code into an executable regression script (18 checks) with 14 replayed real incidents; the replay surfaced 2 holes inherited from the old suite, one of them in the production lint gate |
| Sep 2026 | Alert fatigue treated as a data-model defect — intentional exceptions (athletes with no timed test, by coaching decision) declared as first-class config instead of prose, turning a permanently-red check into a silent one. The deadline to revisit each exception is now carried by the system, not by a line in a doc nobody re-reads |
| Sep 2026 | Fail-fast on unknown CLI flags — a hand-rolled arg parser silently swallowed unrecognized arguments, so a run believed to be a dry-run pushed for real. Unknown flag now exits with the valid list |
| Aug 2026 | Reliability push after a 27-incident week — 4 new gates: step-completion preflight (catches skipped steps, invisible on re-read), historical replay sweep, post-push verification, backfill of stale rows. **Mechanical incident detection 19% → 66%**, each gate validated by replaying the real incident it closes |
| Aug 2026 | Session-matching refactor — explicit category written at generation time + multi-key cascade (exact title → date → weekday), replacing keyword guessing on free-text titles. **Completed sessions never matched: 10.1% → 2.2%**, measured on 15 weeks × 20 athletes. Title-only joins scored 25.6%, worse than the heuristic they replaced: the bench is what kept the wrong design out |
| Aug 2026 | Workout blocks rebuilt from the time series instead of device laps — reads the planned-intensity channel to locate block boundaries, so auto-lap watches no longer report an arbitrary GPS kilometre as an interval pace. Raw vs moving pace separated (up to 88s/km apart on urban runs) |
| Aug 2026 | Claims gate — any statement about an athlete's training history is blocked unless the underlying data point is cited. Written after the same error recurred 5 times in 3 weeks; the gate does not judge truth, it makes the sentence impossible to ship unverified |
| Aug 2026 | Availability collected in-channel — athletes report constraints by replying to their calendar note instead of scattered chat threads. Surfaced to the coach as input; nothing is applied automatically |
| Aug 2026 | Strength-training single source — reference cycle imported once, sessions built by copy with a drift assertion before push, ending the week-to-week drift athletes were spotting before the coach did |
| Jul 2026 | Athlete onboarding at scale — 9 new athletes: questionnaire → personalized program → calendar setup → automated welcome notes with HR zones and watch config |
| Jun 2026 | Idempotent multi-athlete push — pre-fetch planned workouts, skip by (date, title); safe re-runs with zero duplicates after partial failures |
| Jun 2026 | Post-run lap analysis — watch lap data matched to workout structure (warm-up/active/rest blocks) with pace, HR, cadence per block · 89-test regression suite |
| Jun 2026 | Methodology-strict program generation — plans copied verbatim from evidence-based training frameworks, pace zones adapted per athlete |
| May 2026 | CoachRunning TrainingPeaks integration — multi-athlete coach mode, pace zone conversion per athlete, auto-sync to watches |
| May 2026 | Automated weekly review — planned vs done comparison per athlete, next week generation, coach notes pushed to athletes' calendars |
| May 2026 | Session debrief pipeline — fetches training data post-run, inserts lap-by-lap analysis + coaching commentary into coach notes |
| Apr 2026 | CoachRunning eval system — LLM-as-judge on 6 quality dimensions + functional regression tests, auto-run before every delivery |
| Apr 2026 | CoachRunning Google Drive pipeline — questionnaire → Claude → Google Doc delivered to athlete's Drive |
| Mar 2026 | Job Search AI OS — 10+ orchestrated skills: offer analysis, CV tailoring, outreach, interview prep, daily check-in |
| Feb 2026 | PM Knowledge OS — 7-domain knowledge base with article ingestion pipeline, prompt library, and skill-based retrieval |
| Aug 2025 | doc-immo MVP — Claude extracts mortgage document fields, outputs structured synthesis with confidence flags |

---

## Projects

### CoachRunning *(live · private repo)*

Agentic running coach currently in beta with 21 active athletes. Generates personalized programs grounded in evidence-based coaching methodology and pushes them directly to athletes' TrainingPeaks calendars, which auto-sync to their Suunto/Garmin watches. Profiles range from first-time runners to marathon and Hyrox competitors.

**AI infra:**
- **Two-layer eval before every delivery** — functional regression (binary assertions: correct zones, valid JSON, pace bounds per athlete) + LLM-as-judge (0–1 score on 6 dimensions: source conformity, pace accuracy, progression coherence, coaching tone, structure, athlete feedback integration). No output ships without passing both.
- **Human-in-the-loop checkpoints** — coach reviews generated program before push; athletes log RPE after each session; weekly debrief surfaces deviations before next week is generated
- **Measured, not asserted** — every reliability change ships with the number it moved: incident detection 19% → 66%, unmatched sessions 10.1% → 2.2%. A comparison bench decides between designs, including rejecting the intuitive one that scored worse.
- **Metrics:** 21 active athletes · live since April 2026 · 300+ workouts delivered · ~70 sessions and 20 coach notes pushed and verified per weekly cycle

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

### doc-immo *(on hold · private)*

AI system that analyzes mortgage document packages and produces a structured synthesis for brokers. Claude extracts key fields, flags inconsistencies, and outputs confidence-scored results for human review.

**AI infra:**
- **Confidence flags on every extraction** — output marks uncertain fields explicitly; human reviewer focuses effort where the model hedges, not across the full document
- **Build-to-learn first** — extraction reliability validated before any UI is built; no premature productization of an unvalidated model behavior

---

## Stack

Claude Code · TrainingPeaks API · Google Drive API · Statsig (CUPED A/B testing) · LLM-as-judge evals · functional regression testing

---

*Senior PM based in Montréal · bouraoui.kd@gmail.com · [LinkedIn](https://linkedin.com/in/kdbouraoui)*
