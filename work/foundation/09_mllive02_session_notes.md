# ML-LIVE-02 — Intro to ML Systems: What ML Actually Solves Here (w/ Mirza)

*FlyRank ML track — Week 2 session · Mohamed Rabea*

Session: Jul 16, 2026 · workshop format · video material:
[youtu.be/aTxLqzQ5Isg](https://youtu.be/aTxLqzQ5Isg). These are my working notes on the
session's five topics — each mapped to where I've already applied it in this repo, with
receipts — plus what I'm bringing to the live Q&A.

## The session's frame, applied to my own work

**1. AI vs ML vs analytics vs rules — when is a plain rule better?**
My own answer from the data: the hand rule (stale ≥180d AND visible) is *right* — its
target cell runs a 0.94 decline rate — and still useless, because it scores only 17–35
pages and starves a top-50 queue. Rules win when the pattern fits in one sentence; ML
earns its keep when signals interact (staleness is non-monotonic within visibility bands
in my starter-data table — `w02_ml_task_framing.ipynb`, section 5).

**2. The ML loop.**
Frame → contract → baseline → model → validate → act. My repo walks it in order:
ML-02/03 (frame), ML-04 (data contract), ML-07 (baseline), ML-08 (model), ML-09
(validation audit), ML-10 (action playbook). The loop's discipline that surprised me:
the baseline is not a formality — mine (0.64 padded P@50) nearly matches the model on
shallow queues and only loses at depth.

**3. Supervised vs unsupervised.**
Mine is supervised ranking-by-score: label = observed decline. The unsupervised lane
(archetype clustering, Lane 3) answers "what kinds of pages exist", not "which page
first" — wrong shape for my editor's decision, which is why I passed on it in ML-02.

**4. Models that memorize fail in the real world (generalization vs overfitting).**
The sharpest number in my repo: the same random forest scores **0.92** Precision@50
under a random split and **0.70** under client-holdout GroupKFold(5)
(`work/outputs/validation_audit_metrics.json`). That +0.22 is memorization dressed as
skill — the session's core warning, measured on my own data before the session ran.

**5. "Take YOUR search question and map it onto an ML task type."**
Done and submitted as ML-03 (`work/notebooks/w02_ml_task_framing.ipynb`): ranking task,
observed-decline target (proxy limits stated), Precision@50 under client-holdout, one
row = one page, and the one-paragraph frame filled honestly.

## Questions I'm bringing to the live session

1. Our label is same-window ("is declining"), and the honest upgrade is past→future
   windows on the daily facts. At what point in the program is that upgrade *expected*
   rather than a stretch — and is a same-window capstone label acceptable if the limit
   is stated?
2. My logistic regression lost to the random floor (0.51 vs 0.53) while RF cleared it.
   Is a linear model failing on interaction-heavy signals a finding worth a paper
   paragraph, or noise I'm over-reading?
3. When Precision@50 rides on heavily tied scores (depth-2 trees give 4 distinct
   values), what's the house style for tie handling — report a range, or fix a
   tie-break rule up front?

*Watching the replay against these notes; anything the session contradicts gets
corrected here with a dated edit.*
