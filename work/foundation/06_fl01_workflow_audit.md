# FL-01 — AI Workflow Audit and Tool Setup

*FlyRank AI Fluency — Week 1 · Mohamed Rabea*

The audit below maps my real week as it actually looks right now: the FlyRank ML
internship is the center of it, with interview preparation as the second track.
Classification follows the on-boarding framework: **Just me** (judgment or identity —
delegating it would gut the point), **AI-assisted** (I drive, AI drafts or checks),
**AI-led, I review** (AI does the bulk, I verify and own the result).

## The audit table

| # | Recurring task | Class | One-line rationale |
|---|---|---|---|
| 1 | Read each FlyRank assignment brief and decide what "done" means | **Just me** | If I can't restate the bar in my own words, I can't own the work — this is the judgment the internship grades. |
| 2 | Choose my lane / research question and defend it with numbers | **Just me** | The program's own rule: core idea first, AI second. A borrowed question produces a capstone I can't explain. |
| 3 | Run and validate the starter/warehouse notebooks top-to-bottom | AI-assisted | AI helps debug errors fast, but I run every notebook myself and read every output — unverified answers aren't answers. |
| 4 | Write pandas/DuckDB queries against the warehouse | AI-assisted | AI drafts query skeletons; I check grains, windows, and the ×100 rate gotchas the data dictionary warns about. |
| 5 | Hunt leakage / sanity-check model results | AI-assisted | AI is a second pair of eyes ("what could leak here?"), but the final call on what's honest is mine. |
| 6 | Draft capstone paper sections from finished analysis | AI-assisted | AI turns my bullet notes into prose in my voice card; I rewrite anything I wouldn't say out loud. |
| 7 | Write commit messages and submission notes | AI-led, I review | Low-stakes summarization of work already done; I read before it ships. |
| 8 | Format markdown tables / tidy notebook structure | AI-led, I review | Pure mechanics, zero judgment; reviewing the diff takes a minute. |
| 9 | Watch weekly session videos and take notes | **Just me** | The point is what sticks in my head for the live Q&A — outsourced notes teach me nothing. |
| 10 | Track assignment deadlines and portal submissions | AI-assisted | AI keeps the checklist; I confirm every "Submission saved." toast myself. |
| 11 | Rehearse my 5-minute presales pitch (ZINAD interview) | **Just me** | Delivery is the skill being tested — only my mouth can practice it. AI critiques a recording at most. |
| 12 | Tighten the pitch script and slide wording | AI-assisted | AI compresses and punches up wording; the claims and stories stay mine. |
| 13 | Update CV bullets / LinkedIn as internship work ships | AI-assisted | AI drafts metric-first bullets from real outputs; I strip anything inflated. |
| 14 | Daily English polish on outward-facing text (emails, portal answers) | AI-led, I review | Fast, low-risk cleanup; I read for tone so it still sounds like me. |

**Honest count:** 4 × Just me · 7 × AI-assisted · 3 × AI-led.

## Toolkit status (evidenced, not aspirational)

| Tool | Status | Evidence |
|---|---|---|
| Claude (claude.ai) | ✅ Active daily — including Claude Code for the repo work | Claude Project screenshot attached to this submission |
| Anthropic Academy | ✅ Enrolled and **certs complete (100%)** | Portal sidebar "Anthropic Certs 100%" / `/intern/courses` |
| ChatGPT | ❌ Not set up yet | Listed honestly; I'll create it the week a task actually needs a second model, not before |

## Claude Project (configured this week)

One Project, custom instructions pasted in: who I am (ML intern at FlyRank building a
content-refresh scoring capstone), tone (my voice card: direct, concrete, numbers first,
short sentences, honest about limits), current goals (ship the capstone; pass the ZINAD
presales interview), and a standing request to **act as a tutor — question my framing
before improving my wording.** Screenshot attached to this submission.

## Three target tasks for FL-02 → FL-04 (with "done well" defined)

1. **Turn capstone results into a plain-English summary a non-technical reviewer can act
   on.** Done well = ≤150 words, every number traceable to `work/outputs/*.json`, zero
   causal claims, reader knows the one action to take.
2. **Draft submission notes from a finished notebook.** Done well = 3–5 sentences, names
   the deliverable file, states what was verified (ran top-to-bottom), nothing the
   notebook doesn't support.
3. **Tighten the 5-minute presales pitch script.** Done well = ≤600 words, opens with the
   client's problem (not my bio), one concrete FlyRank-style example, lands on a clear
   ask within 5 minutes when read aloud.
