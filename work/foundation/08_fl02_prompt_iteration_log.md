# FL-02 — Prompting Fundamentals on Real Tasks v2: Prompt Iteration Log

*FlyRank AI Fluency — Week 2 · Mohamed Rabea*

**The task (FL-01 target #1):** turn my capstone results into a plain-English summary a
non-technical reviewer can act on. Done well = ≤150 words, every number traceable to
`work/outputs/*.json`, zero causal claims, reader knows the one action.

**Ground truth the prompts must respect** (from `work/outputs/model_metrics.json` and
`baseline_metrics.json`): random forest ≈ **0.70** mean Precision@50 out-of-fold
(GroupKFold(5) by client), hand rule **0.64** padded / starves at 35 scored pages,
random floor ≈ **0.53**, logistic regression **0.51** (an honest negative).

**Model used for iterations:** Claude (Fable 5, via Claude Code). Cross-model note at
the end.

---

## v0 — the naive one-liner (saved, as required)

> **Prompt:** "Summarize my model results."

> **Output (excerpt):** "Your results indicate your model performs better than baseline
> approaches. The random forest achieves the strongest metrics, suggesting ensemble
> methods suit this problem. Consider further hyperparameter tuning to improve
> performance…"

**Why it fails:** no numbers, invents a next step (tuning) I never asked about, and
"performs better" is exactly the vague claim my program bans.

## v1 — technique: **role assignment**

> **Prompt:** "You are a data analyst writing for a non-technical content-team lead.
> Summarize my model results."

> **Output (excerpt):** "Here's what your results mean in practical terms: the model is
> better at picking which pages need attention than the current rule of thumb. Out of
> every 50 pages it flags, about 35 genuinely need work…"

- **Changed:** audience + role.
- **Improved:** plain register, the ≈35-of-50 framing is exactly right for lay readers.
- **Still fails:** "about 35" came from nowhere — it guessed (my 0.70 happens to make it
  right, but it didn't know that). Numbers must come from me.

## v2 — technique: **context and motivation**

> **Prompt:** *(v1 +)* "Numbers you may use, and no others: RF Precision@50 = 0.70
> (client-holdout, 5 folds); hand rule = 0.64 but only scores 35 pages; random floor =
> 0.53; logistic regression = 0.51. Motivation: this decides whether the team adopts the
> ranked queue, so overstating it burns trust."

> **Output (excerpt):** "Recommendation summary: the learned queue puts ~35 truly
> declining pages in every 50 it flags (0.70), versus 0.64 for the current rule — and
> the rule runs out of pages to score after 35. A random list would land ~0.53. The
> simpler logistic model (0.51) did not beat random, so we don't recommend it…"

- **Improved:** every number real and sourced; the logreg negative is stated, not hidden.
- **Still fails:** 210 words, and it drifted into recommending adoption — that's the
  reader's call, not the summary's.

## v3 — technique: **output structure**

> **Prompt:** *(v2 +)* "Structure: (1) one-sentence result, (2) three bullet numbers
> with plain meaning, (3) one-sentence honest limit, (4) the one action. ≤150 words."

> **Output (excerpt):** "**Result:** the learned queue finds more genuinely-declining
> pages per 50 reviewed than the current rule, tested on clients it never saw. •
> 0.70 vs 0.64: seven more right calls per hundred reviews… **Limit:** scores say
> 'review first', not 'refresh will pay off'. **Action:** pilot the queue on next
> week's review list beside the current rule."

- **Improved:** fits the word budget; limit + action forced into existence.
- **Still fails:** "seven more right calls per hundred" mixes up the comparison when the
  rule starves — the caveat needs to be inside the bullet, not lost.

## v4 — technique: **few-shot example**

> **Prompt:** *(v3 +)* one worked example of the exact style I want, taken from my
> ML-09 write-up ("0.92 under a random split is memorization, not skill — the honest
> number is 0.70"), labeled GOOD, plus one BAD example ("our AI outperforms manual
> processes") to define the failure mode.

> **Output (excerpt):** "• 0.70 vs 0.64 — but the rule stops at 35 pages, so past that
> point it isn't 0.64 vs 0.70, it's 'no answer' vs 0.70. • A shuffled list lands at
> 0.53; both beat luck, only one scales. • Logistic regression: 0.51 — reported because
> a negative you hide becomes a bug someone else finds…"

- **Improved:** the starving-rule nuance landed inside the bullet; tone matches my
  voice card without me saying "voice card".

## v5 — technique: **step decomposition**

> **Prompt:** *(v4 +)* "Work in two steps. Step 1: list the claims you intend to make,
> each with its source number. Step 2: only then write the summary. Show both steps."

> **Output (excerpt):** "Step 1 — claims: [1] RF 0.70 P@50 out-of-fold ← model_metrics.
> [2] rule 0.64/starves-35 ← baseline_metrics. [3] floor 0.53 ← baseline_metrics.
> [4] logreg 0.51 honest negative ← model_metrics. [5] no causal claim possible ←
> design. Step 2 — summary: (final 138-word text)…"

- **Improved:** the claim ledger makes the summary auditable — each sentence traces to
  a file. This is the piece I'll reuse everywhere.

---

## Cross-model comparison (honest constraint noted)

The brief asks for Claude vs ChatGPT. **I don't have a ChatGPT account yet** (my FL-01
toolkit table says the same), so the comparison actually run on 2026-07-15 is the final
v5 prompt on **claude.ai web** (in my Project — chat: "Model performance comparison for
ranked queue adoption", screenshot attached) versus **Claude via Claude Code** — same
model family, different harness. That's a consistency check, not a true cross-model
test; when a task genuinely needs a second model family I'll open the ChatGPT account
and re-run v5 as-is (the template is model-agnostic on purpose).

What the two runs actually showed — the specifics:

- **Agreement:** identical 4-line claim ledgers; both respected the number fence
  (nothing invented); both kept the caveat inside the bullet it belongs to; both under
  150 words.
- **Divergence 1 — derived arithmetic.** The web run converted 0.70 into "35 of the top
  50" and stated the limit as "15 of every top-50 pages are still misses." The Claude
  Code run never derived new numbers. Both are defensible; the prompt didn't say
  whether derived arithmetic counts as a "number I may use." **Template fix adopted:**
  say explicitly "derived arithmetic from the allowed numbers is permitted/forbidden."
- **Divergence 2 — which limit gets told.** Web chose the metric-internal limit ("the
  queue speeds review, it doesn't replace it"); Claude Code chose the causal boundary
  ("ordering review ≠ promising recovery"). Different, equally honest limits — meaning
  the template under-specified. **Template fix adopted:** name the limit category you
  want (accuracy limit vs causal limit).
- **Divergence 3 — the action.** Web: "pilot on the next two client batches, compare
  reviewer time." Claude Code: "run beside the current rule for one cycle, count which
  top-20 the team keeps." Web's is more measurable; kept it.

## The reusable template (a stranger could apply)

```
You are a data analyst writing for a non-technical <ROLE>.
Numbers you may use, and no others: <METRIC LIST, each with source file>.
Motivation: <what decision this feeds; the cost of overstating>.
Work in two steps. Step 1: list the claims you'll make, each with its source
number. Step 2: write the summary.
Derived arithmetic from the allowed numbers is <permitted/forbidden>.
Structure: (1) one-sentence result, (2) three bullet numbers with plain meaning
— put caveats inside the bullet they belong to, (3) one honest limit of type
<accuracy limit | causal limit>, (4) the one action, stated measurably. ≤150 words.
GOOD example: <one sentence in your voice>. BAD example: <the hype sentence
you never want>.
```

**What each rung taught me:** role fixes register, not truth (v1); the model will guess
numbers unless you fence them (v2); structure creates honesty slots — limits and actions
appear because the template demands them (v3); examples transmit voice better than
adjectives (v4); making the model show its claim ledger first is the cheapest audit
there is (v5).
