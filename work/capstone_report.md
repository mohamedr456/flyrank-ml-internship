# Capstone Report — Refresh / Content Opportunity Scoring (Lane 2)

- **Author:** Mohamed Rabea
- **Lane:** 2 — Refresh / Content Opportunity Scoring
- **Repo:** https://github.com/mohamedr456/flyrank-ml-internship
- **Paper:** https://mohamedr456.github.io/flyrank-ml-internship/ (also in `submission/paper_url.txt`)
- **Date:** 2026-07-15

## 0. Abstract

Content teams can only refresh a handful of pages a week — which first? I built a
refresh-opportunity score on real multi-client search data (30,000 pages, 32 pseudonymized
clients, trailing-90-day metrics) and validated it honestly: against a transparent hand rule,
under a client-holdout split, with deliberate leakage tests. A random forest fills a 50-page
review queue at 70% observed-declining pages vs 56% for the rule and 54% for random ordering;
the same audit shows a random split would flatter the model to 92% (+0.22 memorization gap).
The output is a ranked action queue with plain-language reason codes — decision-support for a
weekly editorial workflow, not a forecast.

## 1. Problem framing

Unit: one page for one client. Output: a ranked review queue with one reason code and one
action per row. Human action: refresh / metadata rewrite / protect-review. Cost of a wrong
call: wasted editor hours, or a clumsy rewrite losing a ranking a page still holds. ML earns
its place because the signals interact (staleness matters only paired with visibility) and the
honest rule starves at 35 pages — it cannot fill one week's queue.

## 2. Data safety

Starter release only, public-safe by construction: hashed IDs, no client names/URLs/queries.
Rate columns ×100; `avg_position = 0` = no data (explicit `has_position` flag);
missingness-pattern flags instead of blind fills. Excluded with reasons: `trend_direction` /
`trend_pct` / `is_declining_label` (label-derived), product decision flags (learn the old rule,
not the world), AI-referral sessions (too sparse for labels), IDs (grouping/splitting only).
Warehouse contract (`w03`) verified grain, availability (`IS TRUE` filters), and a leak-free
future-window label design on `month=2026-03`. Nothing client-identifying appears in `work/`.

## 3. Baseline

Frozen transparent rule: `days_since_last_update >= 180 AND impressions_90d >= 100`, ranked by
impressions; one reason code (`STALE_VISIBLE`). Same data, same metric, same folds as the
model: P@10 1.0 / P@20 0.90 at the head, 0.743 at its full 35-row depth, **0.560 at queue
depth 50** — where it starves. Receipts: `work/outputs/baseline_metrics.json`.

## 4. Model / analysis

Random forest (300 trees, min_samples_leaf 5, seed 42), probability used as ranking score;
logistic regression reported as the readable first attempt (it loses to random — an honest
negative). Features (9): content_age_days, days_since_last_update, log_impressions_90d,
weighted avg_position + has_position, ctr, word_count + has_word_count, engagement_rate.
Target in one sentence: the page's measured trailing-90-day trend is down (concurrent,
observed decline — not a forecast).

## 5. Evaluation

GroupKFold(5) by `client_id` — the deployment question is "does the queue work on a client the
model never saw?". All numbers out-of-fold: random 0.544 (= base), rule 0.560, logreg 0.512,
**RF 0.700** (AUC 0.653). Errors: hardest false positives are big-reach mid-position pages that
look exactly like the declining profile; fold spread 0.56–0.78 tracks fold base rates
(0.38–0.65). Receipts: `model_metrics.json`, `validation_audit_metrics.json`.

## 6. Interpretation

Importances: log impressions 0.27, avg position 0.18, content age 0.17 — while
days_since_last_update sits at 0.05: the model relearned the Week-4 signal audit's MIXED
verdict on staleness by itself. Negative results reported: logistic regression under-performs
random ordering; the 181–365d staleness bucket reverses the naive story (n=169).

## 7. Recommendation

The `w07` playbook: ranked queue with five reason codes (STALE_VISIBLE → refresh,
UNDER_CLICKING → title/description rewrite, PAGE1_AT_RISK → protect-review, LOW_ENGAGEMENT →
content review, MODEL_FLAG → human first). Guardrails: a human opens every page; no
auto-rewrites/publishing/pruning; five monitoring tripwires (precision decay vs frozen
baseline, score drift PSI, mix shift, reason-code collapse, panel events). Confidence:
directional decision-support at 0.70 top-50 precision vs 0.542 base, client-holdout.

## 8. Reproducibility

Fresh clone → `pip install -r requirements.txt` → run `work/notebooks/` in order: `w03` (needs
an HF read token via CLI cache or Colab secret — never in a cell), `w04`, `w05`, `w06`, `w07`,
`capstone`. Seeds fixed at 42 everywhere. Metrics receipts committed under `work/outputs/*.json`;
figures under `work/outputs/figs/`; queue CSVs regenerate per run and stay out of git (CI
leak-guard). No sealed-holdout claim is made; all evaluation is out-of-fold cross-validation
with the builder cells and metric files committed.

## 9. Acknowledgments & data credit

Built on the FlyRank ML Internship dataset — real, pseudonymized, public-safe search data
provided by [FlyRank](https://flyrank.ai). Thanks to the FlyRank mentors whose session
materials shaped the method.
