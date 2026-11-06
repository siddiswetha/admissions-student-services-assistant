# 06 — Cost & Latency Estimate

## Purpose

To estimate what the assistant costs at real admissions volume, how fast it answers, and why routing matters — so the university can afford it in peak week, not just in a demo.

All numbers in this document are EXAMPLE estimates to show the method. They are not real prices. Model prices change, and your university's query volume will differ. Replace the EXAMPLE inputs with your own counts and current prices before you use this to make a decision.

## Step 1 — Estimate queries per day in peak week

Start from real counts if you have them: help-desk logs, email volume, and phone volume in the busiest admissions week last year.

If you do not have real counts yet, use this EXAMPLE method and label it clearly:

| Input | EXAMPLE value | How to get the real value |
|---|---|---|
| Total queries in peak week (all channels) | 7,000 | Count anonymised emails + calls + walk-ins in the busiest week |
| Queries per day in peak week | 1,000 (7,000 ÷ 7) | Total peak-week queries ÷ 7 |
| Share of queries the assistant could handle | 70% | Share of queries that are routine and covered by approved sources — confirm from the 40-question sample in `docs/02-research-notes.md` |
| Assistant queries per day in peak week | 700 (1,000 × 70%) | Queries per day × share the assistant could handle |

**Your table — fill in with real counts:**

| Input | Your value |
|---|---|
| Total queries in peak week | TBD |
| Queries per day in peak week | TBD |
| Share the assistant could handle | TBD |
| Assistant queries per day in peak week | TBD |

## Step 2 — Cost per query for two model options

Compare two options. Option A is a cheaper, smaller model. Option B is a larger, more capable model.

The exact model names and prices will change. Use current prices from your provider when you fill this in, and record the date you checked them.

### EXAMPLE cost table (illustrative only — do not use these prices as real)

| Item | Option A — cheaper model (simple FAQs) | Option B — larger model (multi-document questions) |
|---|---|---|
| EXAMPLE cost per query | $0.01 | $0.05 |
| What it is used for | Single-source, routine questions — for example, "What is the application deadline?" | Multi-document questions — for example, a question that needs both the fee circular and the scholarship policy |
| EXAMPLE cost per day at 700 queries/day (if all queries used this model) | $7.00 (700 × $0.01) | $35.00 (700 × $0.05) |
| EXAMPLE cost per peak week (7 days) | $49.00 | $245.00 |

**Your table — fill in with current prices:**

| Item | Option A — cheaper model | Option B — larger model |
|---|---|---|
| Model name | TBD | TBD |
| Price checked on (date) | TBD | TBD |
| Cost per query | TBD | TBD |
| Cost per day at your peak volume | TBD | TBD |
| Cost per peak week | TBD | TBD |

## Step 3 — Why route simple FAQs to the cheaper model

Routing means: send each question to the model that fits it, not to the most expensive model every time.

| Question type | Route to | Why |
|---|---|---|
| Simple FAQ — one document, one fact | Option A (cheaper model) | The question is straightforward. A larger model adds cost without adding accuracy. Most admissions volume is this type. |
| Multi-document question — two policies in one question, or a question that needs comparison | Option B (larger model) | The question needs careful reading across documents. Accuracy matters more than saving a few cents. |
| Must-refuse / must-escalate | Escalate to a human — do not spend model cost trying to answer it | No model should guess a decision or reveal personal data. |

### EXAMPLE — what routing saves

Assume 700 assistant queries per day in peak week:

- 80% are simple FAQs (560 queries) → Option A at EXAMPLE $0.01 = $5.60 per day
- 20% are multi-document questions (140 queries) → Option B at EXAMPLE $0.05 = $7.00 per day
- **Total with routing: $12.60 per day**
- **Total if all queries used Option B: $35.00 per day**
- **EXAMPLE saving from routing: $22.40 per day, or $156.80 per peak week**

This EXAMPLE shows the idea, not a real saving. Fill in your own split (simple vs. multi-document) from your 40-question sample and your own prices.

**Routing rule to write down before launch:**

- If the question needs only one approved source and one fact, route to the cheaper model.
- If the question mentions two policies, two dates, a comparison, or a conflict, route to the larger model.
- If the router is unsure, route to the larger model or escalate — do not risk a wrong answer to save cost.
- Log which model answered each question, so cost per success can be measured per route. See `docs/05-metrics.md`.

## Step 4 — Cost per resolved query and cost per success

Cost per query is not enough. A cheap answer that is wrong or escalated still costs staff time.

| Metric | Formula | EXAMPLE (using the routed total above) |
|---|---|---|
| Cost per query | Total cost ÷ total queries | $12.60 ÷ 700 = $0.018 per query |
| Cost per resolved query | Total cost ÷ queries resolved without staff | If 560 of 700 are resolved: $12.60 ÷ 560 = $0.0225 per resolved query |
| Cost per success | Total cost ÷ grounded-correct answers | If 530 of 700 are grounded-correct: $12.60 ÷ 530 = $0.0238 per success |

Fill in your own numbers after Stage 1. Report all three, not just cost per query.

## Latency

| Metric | Target | What affects it |
|---|---|---|
| Median latency | Under 3 seconds | Retrieval speed, model speed, and how much document text is sent to the model |
| p95 latency | Under 5 seconds | The slowest 5% — often long or multi-document questions |

**How to keep latency down:**

1. Retrieve only the relevant passages, not whole documents. Sending a whole prospectus to the model on every question is slow and expensive.
2. Route simple questions to the cheaper, faster model.
3. Cache nothing that could go out of date. A cached fee or deadline that has changed is a critical error waiting to happen. If you cache, cache only the document index, never the answer.
4. Measure latency by route (simple vs. multi-document) separately. A single average hides the fact that hard questions are slow.

**Your latency table — fill in after testing:**

| Route | Median latency | p95 latency | Meets target? |
|---|---|---|---|
| Simple FAQ (cheaper model) | TBD | TBD | TBD |
| Multi-document (larger model) | TBD | TBD | TBD |
| Overall | TBD | TBD | TBD |

## What to decide from this estimate

Before Stage 2, write down:

- [ ] Peak-week queries per day (real count, not EXAMPLE).
- [ ] The simple vs. multi-document split.
- [ ] The two model options, with prices and the date checked.
- [ ] The routing rule.
- [ ] The weekly cost at peak volume, and who pays for it.
- [ ] The latency you actually measured, by route.
