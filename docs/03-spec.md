# 03 — One-Page Spec

## Product

University Admissions & Student Services Assistant (RAG)

## Problem

Thousands of repetitive admissions questions — deadlines, fees, documents, hostel, scholarships — overload staff, produce inconsistent answers, and risk a student missing admission because of one wrong figure.

## Users

- **Primary:** Prospective and admitted students asking admissions questions.
- **Secondary:** Admissions staff who review answers, handle escalations, and keep source documents correct.

## Goal

Answer routine admissions questions correctly, from approved sources only, with a citation and a date for every answer — and hand off to a human whenever the question is outside scope, personal, or uncertain.

## In scope

| In scope | What it means in practice |
|---|---|
| Answer only from approved sources | The assistant may use only the four approved sources below. If the answer is not in them, it must say so and offer a human handoff. It must not answer from general knowledge. |
| Cite source + date for every answer | Every answer names the document and its publication or last-updated date, so the student and staff can check it. |
| Offer human handoff | Every answer makes it easy to reach a human. The assistant escalates automatically for the cases in "Must escalate" below. |
| Cover the top question types | Deadlines, fees, documents, hostel, scholarships, eligibility rules, and academic calendar — as covered in the approved sources. |
| Show "last updated" | The student sees when the source was last updated, so an old circular is not mistaken for a new one. |

## Out of scope

| Out of scope | Why |
|---|---|
| Admission decisions | The assistant must not say whether a student is admitted, rejected, or waitlisted. Only the admissions office decides that. |
| Personal application status | The assistant must not look up or reveal an individual's application, documents received, or payment status. |
| Fee payment | The assistant must not take payment or confirm that a payment went through. It can point to the official payment route in the approved sources. |
| Guarantee of scholarship | The assistant must not promise a scholarship or funding. It can explain eligibility rules and deadlines from the scholarship policy. |
| Unpublished results | The assistant must not share results that have not been officially published. |

## Approved sources — source inventory

Only these four sources may be used. Anything else is not a source for this assistant.

| # | Source document | Owner | How to check it is current |
|---|---|---|---|
| 1 | Admission Prospectus | Admissions Office | Check publication date and version on the cover. If a new prospectus is published, the old one must be retired, not kept alongside it. |
| 2 | Fee Circular | Finance / Admissions Office | Check circular number and date. A newer circular replaces an older one for the same fee. |
| 3 | Academic Calendar | Academic Office | Check the academic year on the calendar. A calendar for a different year must not be used. |
| 4 | Scholarship Policy | Scholarships Office | Check policy version and effective date. Expired policy versions must be retired. |

**Rule:** If two approved sources disagree, the assistant must not pick one silently. It must say there is a conflict, name both sources and dates, and escalate to staff.

**Rule:** If a source is past its expiry or review date, it must not be used until the owner confirms it is still current. See `docs/07-risks-guardrails.md`.

## Must escalate — always hand off to a human

- Personal data or personal application status.
- Admission decision requests ("Am I admitted?" / "Will I get in?").
- Unpublished results.
- Payment confirmation or payment problems.
- A question the approved sources do not cover.
- Conflicting sources.
- A student who is distressed, confused, or asks for a human.
- Any question in a language the assistant cannot answer reliably — escalate rather than guess.

## Success metrics (summary)

Full metrics plan is in `docs/05-metrics.md`.

- **Primary metric:** Resolution rate without staff intervention, with zero critical errors.
- **Pass bar for eval:** At least 92% grounded-correct, 100% safe on must-refuse questions. See `docs/04-eval.md`.
- **Latency:** Median under 3 seconds, p95 under 5 seconds. See `docs/06-cost-latency.md`.

## Non-goals

- This is not a replacement for admissions staff. It is a first line for routine, sourced questions.
- This is not a general chatbot. It does not answer questions outside admissions and student services.
- This is not a payment or application portal.

## Open questions (to resolve before Stage 2)

- [ ] Who owns each source document, and who confirms it is current each week in peak season?
- [ ] What are the official support hours and handoff route for escalations?
- [ ] Which languages must the assistant support at launch, and which can wait?
- [ ] What is the exact kill-switch process if the error rate spikes? See `docs/07-risks-guardrails.md`.
