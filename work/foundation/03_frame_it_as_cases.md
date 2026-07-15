# Frame It as Cases: Work That Speaks for Itself

*FlyRank AI Fluency — Week 2 · Mohamed Rabea*

## Voice card

**Direct. Concrete. Numbers first. Short sentences. Honest about limits.**

(Standing instruction for any AI drafting help: use these five traits; cut anything I
wouldn't say out loud.)

---

## Case 1 — First discovery on a real search warehouse

**The problem.** A 20k-intern program handed me a real multi-client search dataset and
one week to stop being a stranger to it. Tutorials don't prepare you for rate columns
stored ×100 and `avg_position = 0` meaning "no data," not "rank zero."

**What I did.** Ran the full reference pipeline end-to-end (DuckDB aggregation →
scikit-learn), then re-ran it with my own questions: which safe signals actually move
with clicks, and where the data lies to you. I documented every gotcha I hit in the
notebook instead of pretending the first run worked.

**What came of it.** A working first pipeline, executed notebooks with real outputs
committed, and the discovery that trend columns leak future information — which shaped
every later decision. This week is why I trust my own numbers now.

## Case 2 — Choosing a question worth seven weeks

**The problem.** "Do something with ML on search data" is not a research question. Pick
wrong and you spend seven weeks training a model nobody can act on.

**What I did.** Wrote the framing before touching a model: unit of analysis = one page;
decision supported = "which pages does a content team refresh this week?"; cost of a
wrong recommendation = wasted editor hours on healthy pages. Backed the choice with real
numbers from the starter dataset, and committed to a success metric up front:
**Precision@50 under a client-holdout split.**

**What came of it.** Lane 2 — Refresh / Content Opportunity Scoring — chosen as a
provisional lane with a written rationale, not a vibe. The framing notebook is committed
and became the spine of my capstone.

## Case 3 — Turning a lane into a buildable ML task

**The problem.** "Score pages for refresh" still isn't an ML task. What's the label? What
are the features? What split proves it works on a client it has never seen?

**What I did.** Defined the task type (ranking/scoring, not binary classification),
sketched the target as a future-window opportunity signal, restricted features to safe
past-window aggregates (no `trend_*` leakage), and designed validation as grouped,
client-aware splits so the score means something for a new client.

**What came of it.** A written data contract the rest of the capstone builds on: label
definition, feature list, and validation design agreed before modeling starts — the
difference between research and curve-fitting.

---

## Bio

Mohamed Rabea. Data engineering & ML — Cairo University. Currently running three
internships in parallel; the FlyRank ML track is where I prove end-to-end judgment on
real search data: honest labels, leakage-checked features, validated models, and
recommendations a content team can execute.

## Contact / CTA

**Read the capstone paper (live in Week 8), then email me: mhmdrby769@gmail.com** —
I'm looking for a junior ML/data role with real production data.

---

## Before / after (generic AI line vs. mine)

**Generic AI:** "Leveraged cutting-edge machine learning techniques to deliver
data-driven insights and drive impactful business outcomes."

**Mine:** "I score 12 weeks of real search logs to tell a content team which 50 pages to
refresh first — and I validate on clients the model has never seen."
