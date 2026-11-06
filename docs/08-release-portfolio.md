# 08 — Staged Release + Portfolio Write-up

## Part A — Staged release plan

Do not launch to all students at once. Release in stages, and only move forward when the evidence says it is safe.

### Stage 1 — Staff-only

**Who:** Admissions staff only. No students see the assistant.

**What happens:**

- Staff ask the assistant the questions students ask, using real anonymised questions where possible.
- Staff review **100% of answers** — every answer is checked for correctness, grounding, citation, and safe refusal.
- Staff log every failure, especially critical errors and unsafe refusals.

**Exit criteria — all must be true before Stage 2:**

- [ ] The 60-question eval passes: at least 92% grounded-correct, 100% safe on must-refuse, zero critical errors. See `docs/04-eval.md`.
- [ ] Staff have reviewed 100% of Stage 1 answers and are confident in the citation trail.
- [ ] All source documents are current, and the expiry check is working. See `docs/07-risks-guardrails.md`.
- [ ] The kill-switch has been practised and the static FAQ page is ready.
- [ ] The handoff route to a human is tested and staff know who handles escalations.

**If Stage 1 fails:** Stay in Stage 1. Fix the source, routing, or guardrail that failed. Re-run the full 60-question eval.

### Stage 2 — Prospective students, with daily human sampling

**Who:** Prospective students, in a limited rollout.

**What happens:**

- Students can use the assistant for routine questions.
- Staff review a **random 20% sample of answers daily** — not just the easy ones. Include hard, escalated, and multi-document cases.
- Staff track the metrics in `docs/05-metrics.md`: resolution rate, grounded rate, escalation accuracy, staff hours saved, cost per resolved query, and latency.
- Any critical error triggers an immediate review and, if needed, the kill-switch.

**Exit criteria — all must be true before Stage 3:**

- [ ] Live eval stays above the bar for **7 days in a row**: grounded-correct at or above 92% in the daily sample, 100% safe on must-refuse cases, zero critical errors.
- [ ] Escalation accuracy is acceptable to staff — the right questions are reaching humans.
- [ ] Latency meets the target: median under 3 seconds, p95 under 5 seconds.
- [ ] Cost per resolved query is within the budget in `docs/06-cost-latency.md`.
- [ ] Staff hours saved per week is measured and reported, not guessed.

**If Stage 2 fails:** Return to Stage 1, or stay in Stage 2 with a smaller group while you fix the problem. Do not move to Stage 3 to meet a calendar date.

### Stage 3 — Full admissions season

**Who:** All prospective and admitted students during the full admissions season.

**What happens:**

- The assistant is available to all students for in-scope questions.
- Daily sampling continues. The source inventory is checked weekly in peak season.
- The kill-switch remains ready. Any spike in the error rate triggers it.
- A weekly metrics review is held with admissions staff.

**Ongoing checks:**

- Re-run the 60-question eval whenever a source document changes.
- Re-test in every supported language whenever a source changes.
- Keep logging critical errors, unsafe refusals, and student complaints. Investigate every one.

### Rollout decision note — blank template

Fill in one row per stage decision. This is the record a reviewer will ask for.

| Stage | Decision (go / stay / roll back) | Date | Evidence (eval result, sample rate, critical errors) | Decided by | Notes |
|---|---|---|---|---|---|
| Stage 1 → Stage 2 | TBD | TBD | TBD | TBD | TBD |
| Stage 2 → Stage 3 | TBD | TBD | TBD | TBD | TBD |
| Stage 3 — weekly review | TBD | TBD | TBD | TBD | TBD |

**Example of a good decision note (template wording, not a real decision):**

> "On [date], we moved from Stage 1 to Stage 2 because the 60-question eval passed at [X]% grounded-correct with 100% safe refusals and zero critical errors, staff had reviewed 100% of Stage 1 answers, and the kill-switch was practised on [date]."

**Example of a good rollback note (template wording, not a real decision):**

> "On [date], we used the kill-switch because [critical error / error-rate spike]. We showed the static FAQ page, fixed [cause], re-ran the eval, and re-enabled the assistant on [date] after the eval passed."

---

## Part B — Portfolio write-up (Lesson 10.1 style)

Write this section as if you are explaining the project to a hiring manager or a reviewer who has five minutes. Keep it honest: say what you did, what you found, what you decided, and what you would do next. Do not claim results you have not measured.

### The problem

During admissions, a university receives thousands of repetitive questions — deadlines, fees, documents, hostel, scholarships — across email, phone, and walk-ins. Staff are overloaded, answers are inconsistent, and a single wrong deadline or fee figure can cause a student to miss admission.

### What I did

1. **Defined the problem with evidence** — see `docs/01-problem-statement.md`. I stated what I knew, what was EXAMPLE, and what I still needed to prove.
2. **Did user research** — see `docs/02-research-notes.md`. I planned interviews with 5 students and 2 staff, and a method to collect 40 real anonymised questions, identify the top 10 question types, and find the 5 questions where a wrong answer causes the most harm.
3. **Wrote a one-page spec** — see `docs/03-spec.md`. In scope: answer only from approved sources, cite source + date, offer human handoff. Out of scope: admission decisions, personal application status, fee payment, scholarship guarantees.
4. **Built a 60-question eval** — see `docs/04-eval.md` and `evals/eval-set.csv`. 40 typical, 10 hard (two policies in one question, outdated vs. new circular), 10 must-refuse / must-escalate. Rubric: correctness, grounding, citation present, safe refusal. Pass bar: at least 92% grounded-correct, 100% safe on must-refuse.
5. **Planned metrics** — see `docs/05-metrics.md`. Task success, quality, cost per success, and harm — including staff hours saved per week, cost per resolved query, and zero critical errors.
6. **Estimated cost and latency** — see `docs/06-cost-latency.md`. I compared two model options and showed why simple FAQs go to a cheaper model and multi-document questions go to a larger model.
7. **Listed risks and guardrails** — see `docs/07-risks-guardrails.md`. Outdated circulars, hallucinated fees/deadlines, language bias — with source-only answers, an expiry check, "last updated" shown, and a kill-switch to a static FAQ page.
8. **Planned a staged release** — see Part A above. Staff-only first, then prospective students with 20% daily sampling, then full admissions season only if live eval stays above the bar for 7 days.

### What I found

[TBD — write after you run the research and the eval.]

Include, when you have them:

- The top 10 question types, counted from your 40 real questions.
- The 5 highest-harm questions, confirmed by staff.
- Your eval result: grounded-correct rate, safe-refusal rate, and critical errors.
- The failure analysis: what failed, why, and what you changed.
- Staff hours saved per week and cost per resolved query, measured in Stage 2.

Do not write this section from imagination. If you have not run the research or eval yet, leave it as TBD and say so.

### What I decided

[TBD — write after the eval and Stage 1.]

Include the rollout decision: did you move to Stage 2, stay in Stage 1, or roll back — and what evidence supported that decision? Record it in the rollout decision note in Part A.

### What I would do next

[TBD — write after Stage 2.]

Useful next steps to consider:

- Expand language testing to the languages students actually use, based on real enquiry data.
- Improve the router so fewer multi-document questions are sent to the wrong model.
- Reduce staff review time in Stage 2 without lowering the grounded rate.
- Add a feedback loop: when a student reports a wrong answer, trace it to the source and fix the source, not just the answer.

### What I learned

[TBD — write in your own words after completing the project.]

A good closing line names one specific lesson — for example, what surprised you about the research, the eval, or the release decision — and how it changed your approach as a Product Manager.

---

## Portfolio checklist

Before you share this project, check:

- [ ] All 8 docs in `docs/` are complete and consistent with each other.
- [ ] The eval CSV has exactly 60 rows: 40 typical, 10 hard, 10 must-refuse.
- [ ] The eval results table is filled in only with real results, or left as TBD.
- [ ] All EXAMPLE data is labelled EXAMPLE, and no fake quotes are presented as real.
- [ ] The cost table uses current prices and the date they were checked, or is labelled EXAMPLE.
- [ ] The rollout decision note records the real decision and the evidence behind it.
- [ ] No content is copied from any other site, and no other site is mentioned or linked to.
