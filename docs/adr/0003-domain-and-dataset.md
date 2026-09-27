# ADR-0003 — Domain and dataset: consumer lending

- **Date:** 2026-09-27
- **Status:** Accepted
- **Supersedes:** DECISION-001 (healthcare / CMS Medicare), recorded 2026-09-02

## Context

The brief requires choosing an industry and a dataset. The choice constrains
every module, so it needs to satisfy several conditions at once:

1. **Real mess.** Profiling and cleaning are only honest if the defects were
   not planned by the author.
2. **Breadth of column types.** The brief asks for PII detection, date logic,
   mixed-type handling, numeric ranges, categorical consistency and an NLP
   column classifier. A narrow dataset forces most of that to be synthetic.
3. **A time dimension.** Drift detection (VAL-012) needs genuinely different
   distributions across a real axis, not a random split.
4. **Enough scale to justify the engineering.** Chunking, columnar storage and
   a lazy execution engine should be necessary, not decorative.
5. **A domain where data quality is a real, funded concern**, so the project
   reads as product work rather than an exercise.

## Options considered

| Option | Real mess | Column breadth | Time axis | Scale | Verdict |
| --- | --- | --- | --- | --- | --- |
| **Consumer lending** — Lending Club loan book 2007–2018 | High | 145 columns, incl. free text, masked postcodes, money, percentages, dates | 11 years of issue dates | 2.26M rows | **Chosen** |
| Healthcare — CMS Medicare Physician & Other Practitioners | Moderate | Aggregated provider/service level; little free text, no borrower-like PII | Annual files | Large | Superseded |
| Retail — UCI Online Retail II | Moderate | Narrow (~8 columns) | 2 years | 1M rows | Rejected: too thin for the NLP and PII requirements |
| Healthcare — MIMIC clinical data | High | Wide | Yes | Large | Rejected: credentialed access required |
| Synthetic (Faker-generated) | None | Controllable | Controllable | Controllable | Rejected as a primary source: hand-placed defects are visible to a reviewer |

## Decision

**Domain: consumer lending. Primary dataset: Lending Club accepted loans,
2007–2018** — 2,260,668 rows, 145 columns, one row per issued loan.

Source: <https://www.kaggle.com/datasets/wordsforthewise/lending-club>

Secondary dataset for the generalisation test in Module 4: the rejected
applications file from the same source (different schema, larger).

## Why lending over healthcare

The healthcare choice was recorded on 2026-09-02 and is not wrong, but the CMS
dataset is **aggregated** — one row per provider per service, already summarised
by CMS. That works against three of the five conditions above:

- Aggregation removes most row-level defects, so cleaning has less to fix.
- There is no borrower-like personal data and almost no free text, so PII
  detection (PROF-013) and the NLP column classifier (VAL-007) would have to be
  demonstrated almost entirely on injected synthetic columns.
- Provider-level billing totals give weaker cross-field logic than a loan does.

Lending also has a stronger business case for this specific system. Data quality
in lending is regulated in its own right, not just a prerequisite for analytics:
supervisory expectations for risk-data aggregation require that risk data be
accurate, complete and traceable. A system that profiles, cleans, validates,
logs every modification and emits an auditable report maps onto that directly.
Defective data in lending also has a concrete cost — a mispriced rate, a wrongly
approved loan, a credit model trained on corrupted history.

## Consequences

**Positive**

- Real, documented defects exist to find: `emp_length` holds `"10+ years"` and
  `"< 1 year"`; `term` holds `" 36 months"`; dates arrive as `"Dec-2015"`;
  `zip_code` is masked to `"123xx"`; `emp_title` is uncontrolled free text from
  millions of authors; `member_id` is entirely null.
- Genuine cross-field rules are available, including verifying `installment`
  against the amortisation formula given `loan_amnt`, `int_rate` and `term`, and
  checking `sub_grade` against `grade`.
- Eleven years of issue dates give a real drift signal.

**Negative / accepted trade-offs**

- No email or phone columns exist, so the generic email and phone validators
  required by VAL-001 are exercised against injected synthetic PII. This is
  recorded as a deliberate limitation, not hidden.
- The Kaggle distribution was lightly pre-cleaned by its publisher: percent
  signs were stripped from `int_rate` and `revol_util` and those columns cast to
  float. Ample mess remains, and ADR-0004 supplies the evaluation ground truth
  regardless.
- 145 columns is a lot of surface area. ADR-0005 and ADR-0007 address how the
  engine handles all of them while business rules cover a documented subset.

## Follow-up

- Verify the Kaggle licence terms before the file enters version control.
- The file is large and must never be committed directly — see ADR-0009.
