# 07 — Risks & Guardrails

## Purpose

To name what can go wrong, how bad it would be, and what stops it from happening — before students are affected.

A risk list is not a worry list. Each risk below has a likelihood, an impact, a guardrail, and an owner to check.

## Risk table

| # | Risk | Likelihood | Impact | What happens if it happens |
|---|---|---|---|---|
| 1 | Outdated circulars — the assistant answers from an old fee circular or prospectus after a new one is published | High in peak season, when documents change often | High — wrong fee or deadline | A student pays the wrong amount or misses a deadline |
| 2 | Hallucinated fees / deadlines — the assistant invents a figure that sounds plausible but is not in any source | Medium | High — critical error | A student acts on a wrong figure and loses their place or money |
| 3 | Language bias — the assistant answers well in one language but poorly in the languages students actually use | Medium — depends on your student population | High — unequal service | Some students get worse, less safe answers than others |
| 4 | Two sources conflict and the assistant picks one silently | Medium | High | The student gets one answer, staff give another, trust is lost |
| 5 | Personal data revealed — the assistant shares or guesses personal application status | Low if guardrails work; high impact if they fail | Very high — privacy harm and loss of trust | A student's personal information is disclosed |
| 6 | Admission decision guessed — the assistant says a student is admitted or will get in | Medium without a guardrail | Very high | A student makes plans based on a decision that was never made |
| 7 | Over-trust — students and staff stop checking, so a wrong answer spreads before anyone notices | Medium after launch | High | Errors reach many students before they are caught |
| 8 | Source owner unavailable — no one confirms a document is current during peak week | Medium | Medium to high | Outdated answers continue because no one retires the old source |
| 9 | Cost or latency spike in peak week | Low to medium | Medium | Slow answers frustrate students; high cost surprises the university |
| 10 | Kill-switch not tested — the team does not know how to switch to a static FAQ page quickly | Low if tested; high if not | High in an incident | A bad error rate continues while the team works out what to do |

## Guardrails

Guardrails are the controls that prevent a risk from becoming harm. Each one below maps to one or more risks above.

### 1. Source-only answers

- **Rule:** The assistant may answer only from the four approved sources in `docs/03-spec.md`. If the answer is not in them, it must say so and offer a human handoff.
- **How to enforce it:** Retrieval is limited to the approved source set. The assistant is instructed to say "I cannot find this in the approved sources" rather than answer from general knowledge.
- **Prevents:** Risks 2, 5, 6.

### 2. Document expiry check

- **Rule:** Every source has a publication date, a version, and a review / expiry date. A source past its expiry date must not be used until its owner confirms it is still current.
- **How to enforce it:** Before launch, list each source with its dates in a source inventory. Each week in peak season, the owner confirms or retires each source. Expired sources are removed from retrieval, not just marked.
- **Prevents:** Risks 1, 4, 8.

### 3. "Last updated" shown on every answer

- **Rule:** Every answer shows the source name, its date, and "last updated" so the student can see how fresh the information is.
- **How to enforce it:** The answer format requires a citation with a date. Answers without a citation are blocked from being shown. See the rubric in `docs/04-eval.md`.
- **Prevents:** Risks 1, 7.

### 4. Conflict handling

- **Rule:** If two approved sources disagree, the assistant must not pick one silently. It must name both sources and dates, say there is a conflict, and escalate to staff.
- **How to enforce it:** Test this in the hard eval questions (10 questions in `evals/eval-set.csv`). Staff review conflicts in Stage 1.
- **Prevents:** Risk 4.

### 5. Must-refuse guardrail

- **Rule:** For personal data, personal application status, admission decisions, unpublished results, and payment confirmation, the assistant must refuse the disallowed part, explain briefly why, and offer a human handoff.
- **How to enforce it:** Test all 10 must-refuse questions in the eval. The pass bar is 100% safe. One unsafe answer fails the eval.
- **Prevents:** Risks 5, 6.

### 6. Language testing

- **Rule:** Test the assistant in the languages students actually use, not just in English. Do not launch a language you have not tested.
- **How to enforce it:** Ask students and staff which languages are used (see `docs/02-research-notes.md`). Run a sample of the eval questions in each language. If a language fails, escalate those questions to a human rather than answering in that language.
- **Prevents:** Risk 3.

### 7. Human review and sampling

- **Rule:** Stage 1 — staff review 100% of answers. Stage 2 — staff review a random 20% sample daily. See `docs/08-release-portfolio.md`.
- **How to enforce it:** Do not move to the next stage until the live eval stays above the bar for 7 days.
- **Prevents:** Risks 2, 7.

### 8. Kill-switch to a static FAQ page

- **Rule:** If the error rate spikes, the team can immediately switch the assistant off and show a static FAQ page instead.
- **How to enforce it:**
  1. Prepare the static FAQ page before launch, using only the approved sources, with sources and dates shown.
  2. Decide in advance who can trigger the kill-switch, and how — for example, one named person, one action, no approval chain during an incident.
  3. Decide in advance what "error rate spikes" means — for example, any critical error, or grounded-correct rate below 92% in the daily sample.
  4. Practise the switch before launch. Record the date you practised it.
  5. After a kill-switch, fix the cause, re-run the 60-question eval, and record the decision to turn the assistant back on.
- **Prevents:** Risks 1, 2, 7, 10.

### Kill-switch record — blank template

| Date | Triggered by | Reason | What was shown instead | Fix made | Date re-enabled | Eval re-run result |
|---|---|---|---|---|---|---|
| TBD | TBD | TBD | Static FAQ page | TBD | TBD | TBD |

## Source inventory — blank template

Fill this in before launch, and update it every week in peak season.

| Source document | Version / circular no. | Publication date | Review / expiry date | Owner | Status (current / retired) | Last checked |
|---|---|---|---|---|---|---|
| Admission Prospectus | TBD | TBD | TBD | TBD | TBD | TBD |
| Fee Circular | TBD | TBD | TBD | TBD | TBD | TBD |
| Academic Calendar | TBD | TBD | TBD | TBD | TBD | TBD |
| Scholarship Policy | TBD | TBD | TBD | TBD | TBD | TBD |

## What to check before each stage

- [ ] All sources are current, and expired sources have been retired.
- [ ] The 60-question eval passes, including 100% safe on must-refuse.
- [ ] The kill-switch has been practised and the static FAQ page is ready.
- [ ] Language testing is done for every language you plan to support.
- [ ] A named owner exists for each source document.
