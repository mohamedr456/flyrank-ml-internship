# The Prompt Ladder

*FlyRank AI Fluency — Week 2 · Mohamed Rabea*

Real task from my track: **turn my content-refresh model's output into reason codes a
content lead can act on.** This is a task I will do for real in ML-10 (Content Action
Playbook), so the final prompt gets reused.

---

## Baseline (v0) — the embarrassing one

> **Prompt:** "Explain my model results."
>
> **Output (excerpt):** "Your model results show how well your model performs on your
> data. Depending on the metrics used, such as accuracy or F1 score, the results can
> indicate areas of strength and weakness. It's important to consider whether the model
> is overfitting…"

Generic filler. It doesn't know what the model is, who's reading, or what "explain" means.

## v1 — added a clearer goal

> **Prompt:** "I have a model that scores web pages for refresh priority. Write reason
> codes explaining why a page was flagged."
>
> **Output (excerpt):** "Reason codes you could use: 1. DECLINING_TRAFFIC — page has lost
> traffic. 2. OUTDATED_CONTENT — content may be stale. 3. LOW_ENGAGEMENT — users bounce
> quickly…"

- **What changed in the prompt:** named the actual goal (reason codes for refresh flags).
- **What improved in the output:** it stopped lecturing about metrics and produced actual
  reason-code candidates.
- **What still failed:** codes are invented from nothing — my model's features aren't
  "outdated content," they're click/impression/position signals.
- **Next:** give it the real feature space.

## v2 — added real context

> **Prompt:** *(v1 +)* "The model uses weekly search signals per page: impressions,
> clicks, CTR, average position, and their 8-week trends. A page is flagged when its
> predicted opportunity score is in the top 50. Ground every reason code in these signals
> only."
>
> **Output (excerpt):** "RISING_IMPRESSIONS_FLAT_CTR — impressions grew ≥30% over 8 weeks
> while CTR stayed flat, suggesting the snippet under-sells the page.
> POSITION_SLIP — average position declined ≥2 spots from its 8-week peak…"

- **What changed:** injected the real feature space and the top-50 decision rule.
- **What improved:** every code now traces to a measurable signal — these could ship.
- **What still failed:** written for me, not for a content lead; jargon like "CTR" and
  "average position" is unexplained.
- **Next:** define the audience.

## v3 — defined the audience

> **Prompt:** *(v2 +)* "The reader is a content lead with no analytics background. Each
> reason code needs a plain-English one-liner telling them what to do."
>
> **Output (excerpt):** "RISING_DEMAND, SAME_HEADLINE — 'More people are seeing this page
> in search, but the same share click it. Rewrite the title and description.'…"

- **What changed:** one named reader plus a "what to do" requirement.
- **What improved:** each code now pairs diagnosis with an action a non-analyst can take.
- **What still failed:** output rambled — some codes got three sentences, some got one,
  and there was no cap on how many codes exist.
- **Next:** pin the format.

## v4 — specified the output format

> **Prompt:** *(v3 +)* "Format: a table with columns CODE | SIGNAL RULE | PLAIN-ENGLISH
> ACTION (max 15 words). Exactly 6 codes covering: growing, declining, recovering,
> under-clicked, slipping position, stale winner."
>
> **Output (excerpt):** A clean 6-row table, e.g. "SLIPPING_POSITION | avg position down
> ≥2 vs 8-week best | 'Competitors moved above you — update facts and refresh internal
> links.'"

- **What changed:** exact table schema and a fixed taxonomy of six situations.
- **What improved:** output became directly pasteable into the playbook; nothing to trim.
- **What still failed:** honestly — very little. This version was already usable.
- **Next:** try quality criteria anyway and see if it earns its place.

## v5 — added quality criteria / verification

> **Prompt:** *(v4 +)* "Before answering, check each code against: (a) is the signal rule
> computable from impressions/clicks/CTR/position alone? (b) could two codes fire on the
> same page — if so, state the tiebreak. Flag any code that fails."
>
> **Output (excerpt):** Same table, plus: "Note: RECOVERING and RISING_DEMAND can both
> fire; tiebreak: RECOVERING wins if the page was in decline for ≥4 of the last 8 weeks…"

- **What changed:** made the model audit its own codes before output.
- **What improved:** it caught a real overlap (recovering vs. growing) I hadn't specified —
  that tiebreak goes straight into ML-10.
- **What didn't help:** the "flag failures" instruction added a paragraph of hedging on an
  otherwise clean answer. One layer earlier (v4) was already sufficient for format; v5
  earned its place only through the overlap check. **The honest note: v5's verbosity made
  the output worse to paste, even as it made the content safer.**
- **Next:** merge v4's format discipline with only the overlap-check part of v5.

---

## Final reusable prompt

> You are helping a machine-learning intern turn model output into an action playbook.
> The model scores web pages weekly for refresh priority using only: impressions, clicks,
> CTR, average position, and their 8-week trends; the top 50 pages get flagged.
> Produce exactly 6 reason codes as a table: CODE | SIGNAL RULE | PLAIN-ENGLISH ACTION
> (max 15 words), covering growing, declining, recovering, under-clicked, slipping
> position, and stale-winner situations. The action column is written for a content lead
> with no analytics background. Every signal rule must be computable from the listed
> signals alone. If two codes can fire on the same page, state the tiebreak in one line
> below the table.

Anyone on the ML track could run this without me in the room.
