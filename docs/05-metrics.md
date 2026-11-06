# 05 — Metrics Plan

## Purpose

To measure whether the assistant actually helps students and staff — not just whether it replies quickly or sounds fluent.

A metric is only useful if you know how to measure it, what good looks like, and what you will do if it gets worse.

## The four metric groups

This project tracks four groups: task success, quality, cost per success, and harm.

### 1. Task success — did the student get what they needed?

| Metric | Definition | How to measure it | Target (to confirm after Stage 1) |
|---|---|---|---|
| Resolution rate without staff intervention | Share of questions fully and correctly answered by the assistant, with no staff help needed | Count resolved queries / total queries, confirmed by staff sampling and follow-up | TBD — set after Stage 1 baseline; the eval pass bar is ≥92% grounded-correct |
| Escalation rate | Share of questions handed off to a human | Count escalations / total queries | TBD — a high rate is not automatically bad if escalations are correct |
| Escalation accuracy | Share of escalations that were correct — the question really did need a human | Staff review of a sample of escalations each day | Target: high; a wrong escalation wastes staff time, a missed escalation risks harm |
| Re-ask rate | Share of students who ask the same question again after an assistant answer | Track repeat questions in the same session or within 24 hours | Lower is better; a high rate suggests unclear or incomplete answers |

**What success looks like:** Students get a correct, complete answer the first time, and only the questions that truly need a human reach staff.

### 2. Quality — was the answer correct, grounded, and safe?

| Metric | Definition | How to measure it | Target |
|---|---|---|---|
| Grounded rate | Share of answers where every factual claim is supported by a cited approved source | Human review of a sample + the 60-question eval in `docs/04-eval.md` | ≥92% grounded-correct in eval |
| Citation rate | Share of answers that include a source and a date | Automatic check: does the answer contain a citation? | 100% — every answer must cite |
| Safe refusal rate | Share of must-refuse questions safely refused or escalated | Eval + live sampling of refusal cases | 100% |
| Critical error rate | Number of wrong deadline / fee / eligibility / document answers that could cause a student to miss admission or lose money | Staff review + student reports + eval | 0 |
| Staff hours saved per week | Hours staff no longer spend on routine questions the assistant resolved | (Average minutes per routine query × resolved queries per week) − review and escalation time | TBD — measure in Stage 1 and Stage 2, report as hours per week |

**How to measure quality in practice:**

- **Stage 1 (staff-only):** Staff review 100% of answers. Record grounded / not grounded, citation present / missing, safe / unsafe.
- **Stage 2 (20% sampling):** Staff review a random 20% sample daily. Do not only review the easy ones — include hard and escalated cases.
- **Always:** Log every answer, its citation, and its source date, so a failure can be traced back.

### 3. Cost per success — what does a good answer cost?

| Metric | Definition | How to measure it | Why it matters |
|---|---|---|---|
| Cost per query | Total cost to answer one question, including model cost and retrieval cost | Total cost / total queries — see `docs/06-cost-latency.md` | Shows whether the service is affordable at peak volume |
| Cost per resolved query | Cost per question resolved without staff help | Total cost / resolved queries | A cheap assistant that does not resolve questions is not cheap |
| Cost per success | Cost per grounded-correct, safely handled answer | Total cost / grounded-correct answers | The honest cost metric — it counts only answers that were actually good |

**Rule:** Report cost per success alongside cost per query. Cost per query alone hides failures: an assistant can be cheap per query and expensive per success if many answers are wrong or escalated.

See `docs/06-cost-latency.md` for the cost table and the routing decision (cheap model for simple FAQs, larger model for multi-document questions).

### 4. Harm — what went wrong, and how badly?

Harm metrics are the most important group in this project, because a wrong admissions answer can cause a student to miss admission.

| Metric | Definition | How to measure it | Target |
|---|---|---|---|
| Critical errors | As defined above — wrong deadline, fee, eligibility, or document | Staff review, student complaints, eval failures | 0 |
| Unsafe refusals | Must-refuse questions answered instead of refused, or refused without a handoff | Eval + live sampling | 0 |
| Outdated-source answers | Answers that cite an expired or superseded document | Expiry check log + staff review | 0 |
| Student complaints about wrong information | Students who report they acted on a wrong answer | Complaint log, tagged by topic | 0 critical; investigate every report |
| Time to fix a wrong source | How long it takes, from discovery to correction, to fix a wrong or outdated source | Incident log | As fast as possible; define an owner and a target before launch |

**Harm review process:**

1. Log every critical error or unsafe refusal the same day it is found.
2. Note the question, the answer, the source cited, and the correct answer.
3. Decide the fix: retire a source, correct a document, change routing, or add a guardrail.
4. Re-run the eval before returning to normal service.
5. If the error rate spikes, use the kill-switch. See `docs/07-risks-guardrails.md`.

## Latency — how fast is fast enough?

| Metric | Target |
|---|---|
| Median latency | Under 3 seconds |
| p95 latency | Under 5 seconds |

Median means half of answers are faster than this. p95 means 95% of answers are faster than this — so only 5 in 100 are slower.

Measure latency from when the student sends the question to when the full answer is shown. If latency is high, check retrieval first — a slow or broad document search is a common cause — before assuming the model is the problem. See `docs/06-cost-latency.md`.

## Metrics dashboard — blank template

Fill this in weekly during the admissions season. Do not invent numbers.

| Week starting | Total queries | Resolved without staff | Resolution rate | Grounded rate (sample) | Escalations | Critical errors | Median latency | p95 latency | Cost per resolved query | Staff hours saved |
|---|---|---|---|---|---|---|---|---|---|---|
| TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD |
| TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD | TBD |

## What to do when a metric gets worse

- **Grounded rate falls:** Pause wider rollout. Check whether a source changed, expired, or conflicts with another source.
- **Escalation accuracy falls:** Review whether the assistant is escalating too much (wasting staff time) or too little (risking harm). Adjust the escalation rules, not just the model.
- **Cost per success rises:** Check whether the larger model is being used for simple questions that the cheaper model could handle. See `docs/06-cost-latency.md`.
- **Critical errors appear:** Use the kill-switch if needed, fix the source, re-run the eval, and record the incident in `docs/08-release-portfolio.md`.
