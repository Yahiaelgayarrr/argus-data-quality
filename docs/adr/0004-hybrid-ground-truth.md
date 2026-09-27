# ADR-0004 — Hybrid real data plus injected ground truth

- **Date:** 2026-09-27
- **Status:** Accepted

## Context

Module 3 requires precision, recall, a confusion matrix and ROC curves
(VAL-016). Every one of those metrics needs **labels**: for each row or cell, a
known answer as to whether it is actually defective.

A real dataset does not come with those labels. Nobody has annotated which of
Lending Club's 2.26M rows contain errors. Without labels, the only available
evaluation is eyeballing flagged rows and asserting they look wrong, which is not
measurement.

Conversely, a purely synthetic dataset has perfect labels but no credibility: the
defects are the ones the author thought to create, so detection performance says
more about the author's imagination than about the system.

## Options considered

- **Real data only.** Honest mess, no metrics. Fails VAL-016.
- **Synthetic data only.** Perfect metrics, unconvincing mess. Detection scores
  would be near-meaningless.
- **Hybrid: real data as the substrate, with a controlled corruption layer that
  records every injected defect.**

## Decision

Use the **hybrid** approach.

1. Profiling and cleaning (Modules 1–2) run against the **real** data, so they
   face defects nobody planned.
2. A **corruption injector** takes a clean slice of the real data, applies
   defects of known type and location, and writes a label file recording every
   change: row index, column, original value, corrupted value, defect class.
3. Module 3 is evaluated against that label file.

The injector is built **before** the detectors it evaluates, so that detector
design cannot be quietly tuned to the specific defects present.

## Defect classes to inject

Each class maps to a requirement it is there to evaluate:

| Class | Example | Evaluates |
| --- | --- | --- |
| Out-of-range numeric | negative `loan_amnt`, `dti` of 9000 | VAL-002, VAL-010 |
| Broken date format | `"2015-13-45"`, `"Dec-15"` vs `"Dec-2015"` | VAL-010, CLEAN-008 |
| Column swap | `annual_inc` and `loan_amnt` contents exchanged | VAL-007, VAL-008 |
| Category typo | `"Fully Paid"` → `"Fully Pai"`, `"FULLY PAID"` | VAL-003 |
| Cross-field violation | `earliest_cr_line` after `issue_d` | VAL-010 |
| Near-duplicate row | row copied with small perturbations | CLEAN-007 |
| Injected synthetic PII | Faker-generated email / phone columns | VAL-001, PROF-013 |
| Masked value corruption | `zip_code` inconsistent with `addr_state` | VAL-001 |
| Missingness injection | values blanked where the true value is known | CLEAN-002 to CLEAN-006 |

The last class does double duty: because the true value is retained in the label
file, imputation methods can be compared by how close they get, rather than by
assertion.

## Guard against leakage

- Injection parameters live in a config file, separate from detector config.
- Label files are written to a directory the detectors do not read.
- A detector must never take the injection seed or class list as input.
- The corruption run used for final reported metrics uses a seed not used during
  development.

## Consequences

- Reported detection metrics measure performance **against the injected defect
  distribution**, which is not necessarily the real-world defect distribution.
  Every reported figure must be stated with that caveat.
- Real defects found by profiling in Module 1 are reported qualitatively, since
  they have no labels. The two evidence types are kept clearly separate.
- Building the injector is real work that produces no user-facing feature. It is
  nonetheless the component that makes Module 3's numbers mean anything.
