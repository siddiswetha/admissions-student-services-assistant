# 01 — Problem Statement

## Project

University Admissions & Student Services Assistant (RAG)

## The problem, in one sentence

During admissions, a university receives thousands of repetitive questions, staff are overloaded, answers are inconsistent, and a single wrong deadline or fee figure can cause a student to miss admission.

## Who is affected

**Prospective and admitted students**

- They need a correct answer quickly, often before a deadline.
- They ask the same questions in different places — email, phone, walk-ins — and get different answers.
- If they act on a wrong date or fee, they may lose their place, pay the wrong amount, or miss a document deadline.

**Admissions staff**

- They answer the same questions again and again during peak weeks.
- They work under time pressure, so mistakes and shortcuts happen.
- They have less time for the cases that really need a human: complex eligibility, special circumstances, and distressed applicants.

**The university**

- Inconsistent answers damage trust.
- Wrong answers create complaints, rework, and in the worst case, a student who was eligible but missed out.

## Why this is not just a chatbot demo

A demo chatbot is judged on whether it sounds fluent. This assistant is judged on whether it is right.

A fluent wrong answer about a fee or a deadline is worse than no answer. That is why the whole design in this project is built around grounding, citations, expiry checks, and human handoff — not around sounding clever.

## Evidence — what we know, and what we still need to prove

This section states the evidence honestly. Items marked EXAMPLE are illustrative, not real data. Replace them with your own research.

### Evidence we can start from (EXAMPLE — to be replaced by real counts)

| Evidence | What it suggests | Status |
|---|---|---|
| Admissions receives thousands of repetitive queries in peak weeks across email, phone, and walk-ins | Demand is high and concentrated, not spread evenly through the year | EXAMPLE — confirm with help-desk logs |
| The same question types repeat: deadlines, fees, documents, hostel, scholarships | A small set of topics covers most of the volume | EXAMPLE — confirm from 40 real questions in `docs/02-research-notes.md` |
| Staff report overload and inconsistent answers | The current process depends on individual memory and availability | EXAMPLE — confirm in staff interviews |
| A wrong deadline or fee can cause a student to miss admission | Errors are not minor; they are high-stakes | Reasoning from the problem — confirm with real cases in research |

### Evidence we do NOT have yet (to collect)

- [ ] Actual query volume per week in peak season, by channel (email / phone / walk-in).
- [ ] The true top 10 question types, counted from anonymised enquiry emails, FAQs, and help-desk logs.
- [ ] The 5 questions where a wrong answer causes the most harm, confirmed by staff.
- [ ] How long staff currently spend per query, and how many hours that adds up to per week.
- [ ] How often students receive conflicting answers today.

Do not present the EXAMPLE rows above as findings. They are placeholders that show what good evidence looks like.

## What "good" looks like

A student asks: "What is the last date to submit documents for admission?"

A good answer:

1. Gives one date, not a range of guesses.
2. Names the source document and its date — for example, "Admission Prospectus 2026–27, published [date]".
3. Says what to do if the document is unclear, and offers a human handoff.
4. Does not add information from outside the approved sources.

A bad answer gives a date with no source, mixes two different years, or guesses when the document is silent.

## Scope in brief

Full scope is in `docs/03-spec.md`. In short:

- **In scope:** Answer only from approved sources, cite source + date for every answer, offer human handoff.
- **Out of scope:** Admission decisions, personal application status, fee payment, scholarship guarantees.

## Primary metric

**Resolution rate without staff intervention, with zero critical errors.**

- Resolution rate = the share of questions the assistant answers fully and correctly without staff help.
- Zero critical errors = no wrong deadline, fee, eligibility, or required-document answer that could cause a student to miss admission or lose money.

If resolution is high but critical errors are not zero, the project has failed.

## Success criteria

The project succeeds if, after the eval in `docs/04-eval.md`:

- At least 92% of answers are grounded-correct (correct and supported by a cited source).
- 100% of must-refuse questions are safely refused or escalated.
- Staff can review answers in Stage 1 and trust the citation trail.
- The staged release in `docs/08-release-portfolio.md` can be defended with data, not optimism.

## What this document does not claim

- It does not claim a real university has adopted this assistant.
- It does not claim the EXAMPLE evidence above is real.
- It does not claim any eval result — the eval has not been run yet. See `docs/04-eval.md`.
