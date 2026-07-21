# Kill Your Darlings — Curate Your Images

*FlyRank AI Fluency — Week 3 · Mohamed Rabea*

The rule for this set: **the work shows itself.** Every image that carries proof is a real
capture — a screenshot of the shipped paper or a real output figure from my own pipeline.
AI was allowed only for one thing: the connective brand mark. Nothing that a hiring manager
reads as evidence was generated.

Image set lives in `work/foundation/portfolio_images/`. It maps to the content map in my
Through-Line (`04_through_line.md`): Landing hero, Work/results, About structure.

## The keepers (5) — what each is, and the real-vs-AI call

| # | File | What it is | Real capture or made? | Why this call |
|---|---|---|---|---|
| 1 | `cap_01_paper_hero.png` | Live screenshot of my deployed capstone paper (hero + abstract + problem) | **Real screenshot** | This is the single most important image on the site. A generated "research paper mockup" would be a lie about a thing that actually exists at a public URL — the real capture *is* the proof. |
| 2 | `cap_02_results_p50.png` | The headline result chart: Precision@50 by method (random 0.54 · rule 0.56 · logistic 0.51 · random forest 0.70), client-holdout | **Real output** (matplotlib, from `scripts`/notebooks) | The whole claim rests on these four bars. Every number traces to `work/outputs/*.json`. A prettied-up AI chart would break that chain — an unverifiable number is worse than an ugly one. |
| 3 | `cap_03_feature_importances.png` | What the model actually learned — feature importances | **Real output** | Shows the model is readable, not a black box. Must be the true importances or it misrepresents the model. |
| 4 | `cap_04_portfolio_sitemap.png` | The 4-page portfolio structure (Landing → Work → About → Contact → one action) | **Real diagram** — hand-authored SVG, my own layout | It's my own information design, not decoration. Rendered from SVG I wrote, so it's crisp at any size and edits in seconds. |
| 5 | `brand_monogram.svg` | The "MR" monogram — the one piece of connective tissue | **Made, deliberately** — hand-written SVG in my identity-kit style (Space Grotesk, near-black on warm white, teal underline) | This is the only place generation/authoring is right: a small brand mark that must hold one consistent style across favicon, header, and repo avatar. It carries no claim, so making it is honest. |

**Consistency check:** every image sits in the same kit — deep teal `#0F766E`, near-black
`#111827`, warm near-white `#FAFAF9`. The figures, the paper, the sitemap, and the monogram
read as one set, not a pile.

## Where a real capture beats AI (the point of the assignment)

Items 1–3 are the test. The temptation with AI is to generate a glossy "dashboard hero" or
a slick chart. I chose real captures for all three because **the value of this portfolio is
that the work is real and verifiable in five minutes.** A generated stand-in would look
nicer and prove nothing — and the first time a reviewer clicked through and found the real
thing didn't match, I'd lose the only thing I'm selling: that my numbers are honest.

## The rejection note (graded discernment)

**What I generated and rejected:** a hero image — an AI-generated "analyst at a wall of
glowing dashboards, teal accent lighting." It matched my palette and looked expensive.

**Why I killed it:** it depicts a fiction. There is no wall of dashboards in this project;
there's a 30,000-row CSV, a DuckDB query, and four honest bars. The generated hero would
set an expectation of scale and polish the actual work doesn't claim — and my whole thesis
is *honest, decision-support, no overstating.* An image that oversells is the visual version
of the exact failure my paper warns about (the 0.92 random-split number that looks amazing
and means nothing). The real `cap_01_paper_hero.png` is less flashy and infinitely more
persuasive, because a reviewer can click the URL and land on the very thing in the picture.

**Second, smaller rejection:** an AI-generated set of "feature icons" (little gradient
glyphs for each section). Rejected — they'd be a fifth style competing with the four real
captures. One monogram is enough connective tissue; more decoration would turn a set into a
pile.

## The one image I did NOT make, on purpose

The About page needs a photo where **the subject is me.** The assignment is explicit: use a
real photo where the subject is the person — and generating a synthetic portrait of a real
individual would be dishonest and defeats the point. So that single asset is intentionally
left for a real photo Mohamed supplies himself; I will not fabricate a person. It's the one
"keeper" the image set can't manufacture, and that's the correct call.
