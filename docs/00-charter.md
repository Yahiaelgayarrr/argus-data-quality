# Project Charter — Argus

> **Argus** — Automated Data Quality & Validation System
> *Profile · Clean · Validate · Automate*

This document defines what the project is for, who it serves, what it will and
will not do, and how its success is judged. It is the first document to read and
the one against which scope disputes are settled.

Named for the hundred-eyed watchman of Greek myth, who was set to guard what
mattered and never closed all his eyes at once.

---

## 1. The problem

Data arriving at an organisation is rarely fit to use. It has missing values that
sometimes mean "unknown" and sometimes mean "not applicable". It has the same
category spelled four ways. It has dates in three formats, numbers stored as
text, duplicate records that are not byte-identical, and columns whose contents
do not match their names. Occasionally it has personal data in a column nobody
labelled as personal.

None of this is exotic. What makes it expensive is that it is **invisible** at the
point of use. A model trains successfully on corrupted data. A dashboard renders a
confident number from a broken join. The defect surfaces later, as a wrong
decision, and by then the chain back to its cause is cold.

The usual response is manual inspection: an analyst opens the file, looks around,
fixes what they notice, and moves on. This does not scale, is not repeatable, and
leaves no record of what was changed or why.

## 2. What Argus is

A **data quality gatekeeper**: an automated engine that stands between incoming
data and the systems that consume it.

For each dataset it receives, Argus:

1. **Profiles** it — infers what each column actually is, measures completeness
   and cardinality, finds distributional oddities, and flags columns that are
   suspicious or that appear to hold personal data.
2. **Cleans** it — normalises formats, resolves duplicates, and handles missing
   values using a strategy chosen per column rather than one applied blindly,
   **recording every modification it makes**.
3. **Validates** it — applies declared business rules, runs machine-learning
   anomaly detection, checks whether column contents match their declared
   meaning, and tests whether the data has drifted from an earlier reference.
4. **Scores** it — emits a health score with the evidence behind it, so that a
   dataset can be accepted, accepted with warnings, or rejected.

It runs end-to-end from a single command, is driven by configuration rather than
code changes, and produces machine-readable reports that downstream systems and
humans can both consume.

## 3. The domain: consumer lending

The system is built and demonstrated against a real consumer lending dataset —
the Lending Club loan book, 2007–2018, 2.26M loans across 145 columns
(see [ADR-0003](adr/0003-domain-and-dataset.md)).

Lending is chosen because data quality there is not merely a prerequisite for
good analytics; it is **a regulated concern in its own right**. Supervisory
expectations for risk-data aggregation require institutions to demonstrate that
risk data is accurate, complete, and traceable to its source. A defect does not
produce a misleading chart — it produces a mispriced interest rate, a wrongly
declined applicant, or a credit model trained on corrupted history.

That gives every component of this system a concrete reason to exist:

| Argus capability | Why lending needs it |
| --- | --- |
| Audit log of every modification | Cleaning decisions must be defensible after the fact |
| PII detection | Borrower data carries disclosure obligations |
| Cross-field validation | An instalment that disagrees with its own amortisation terms is a real defect |
| Drift detection | Lending standards shift; a model trained on 2010 borrowers is not valid for 2016 ones |
| Anomaly detection | Fraud and data-entry error look similar and both matter |
| Health score with evidence | Someone has to decide whether this batch is usable |

**The engine itself is domain-agnostic.** Lending knowledge lives in a separable
rule pack. This is verified in Module 4 by running the finished pipeline on an
unrelated dataset with only a config change ([ADR-0006](adr/0006-package-first-architecture.md)).

## 4. Who it serves

Three users, with genuinely different needs. Design decisions are settled by
asking which of them is affected.

### The Data Engineer — primary operator

Runs Argus on incoming batches, from a terminal or a scheduled job.

- **Needs:** one command; a clear pass/fail signal; an exit code usable in CI;
  logs that name the file, column and rule when something fails.
- **Does not need:** charts, or a browser.
- **Failure that matters to them:** a run that fails ambiguously, or that passes
  data it should have stopped.

### The Data Scientist — primary reporting user

About to build a credit-risk model on this data, and needs to know what they are
working with.

- **Needs:** what is in the dataset; what is broken; **what was changed and why**;
  which columns are trustworthy; whether this batch resembles the data a model
  was trained on.
- **Cares particularly about:** imputation choices, since a silently
  mean-imputed column will quietly distort their model, and about which columns
  are post-origination and therefore leak.
- **Failure that matters to them:** a cleaner that improves the data without
  saying how.

### The Risk / Compliance Reviewer — secondary, read-only

Does not write code. Must nonetheless be able to see and sign off on data quality.

- **Needs:** a health score in plain language; an audit trail of modifications;
  flagged personal data; an exportable report.
- **Failure that matters to them:** a system whose conclusions can only be
  inspected by reading Python.

This third persona is the reason the project includes a reporting layer beyond the
CLI ([ADR-0008](adr/0008-interface-sequencing.md)). It is also a genuine
requirement in regulated lending, not a convenience.

## 5. In scope

**Engine**

- Tabular ingestion: CSV and Parquet
- Full profiling: physical and semantic type inference, mixed-type detection,
  missingness structure, cardinality, distributions, correlation, suspicious- and
  PII-column flagging
- Cleaning: format and type normalisation, exact and fuzzy duplicate resolution,
  missing-value handling with statistical and ML imputation, and a separate
  ML-ready transformation output
- Validation: declarative rule engine, Isolation Forest and Local Outlier Factor
  anomaly detection, an autoencoder detector, an NLP column-meaning classifier,
  and drift detection
- Quality scoring before and after cleaning, with a delta, and a dataset health
  score after validation
- A complete audit log of every modification

**Evaluation**

- A corruption injector producing labelled ground truth
  ([ADR-0004](adr/0004-hybrid-ground-truth.md))
- Detection measured by precision, recall, confusion matrix and ROC
- Imputation methods compared against known held-out true values

**Engineering**

- An installable Python package with a stable public API
- CLI; self-contained HTML reports; FastAPI service; React dashboard — in that
  order, with the last two cuttable
- YAML-driven configuration with schema validation
- Structured logging, DVC data versioning, Docker, pytest, GitHub Actions CI
- A generalisation run on a second, unrelated dataset

## 6. Out of scope

Stated explicitly so that scope creep has to argue its case.

| Excluded | Why |
| --- | --- |
| Streaming / real-time ingestion | Batch is the problem being solved |
| Database write-back connectors | Argus reports on data; it does not own the warehouse |
| Enterprise data-catalog integration | No catalog to integrate with |
| Paid cloud deployment | Free tools only, per the brief |
| Arbitrary file formats at production scale | CSV and Parquet cover the need |
| Training or fine-tuning large language models | Pre-trained embeddings suffice for column classification |
| Automatic repair of semantic errors | Argus flags a column swap; it does not unswap it. Silent structural repair is more dangerous than a loud warning |
| Committing real personal data | Never, under any circumstances |
| Multi-user accounts, auth, RBAC | Single-operator tool |

## 7. Success criteria

The project is complete when all of the following hold. Each is checkable, not a
matter of opinion.

**Functional**

1. A single command runs profiling → cleaning → validation end-to-end on the full
   2.26M-row dataset and writes all required artifacts.
2. `profiling_report.json`, `cleaned_data.csv`, `cleaning_log.json` and
   `validation_report.json` are produced and conform to documented schemas.
3. Every modification made during cleaning is reconstructable from
   `cleaning_log.json` alone.
4. The pipeline runs on the second, unrelated dataset with only configuration
   changes — no code edits.

**Measured**

5. Imputation strategies are compared on held-out true values, with results
   recorded.
6. Rule and anomaly detection are reported with precision, recall, confusion
   matrix and ROC against injected ground truth, with the caveats of that method
   stated.
7. Quality scores before and after cleaning are reported with the delta.

**Engineering**

8. `pytest` passes, and CI runs it on every push.
9. `docker run` reproduces a pipeline run on a machine with no GPU and no
   pre-installed Python environment.
10. Every requirement ID in [`01-brief-traceability.md`](01-brief-traceability.md)
    is either `IMPLEMENTED`/`VERIFIED`, or is `DEFERRED` with a recorded reason.

**Documentation**

11. One document per module explaining what was built, why those techniques, and
    what the results were.
12. Every significant technical choice has an ADR stating the alternatives
    considered.
13. A reader can clone the repository and understand the system without asking
    the author anything.

## 8. Constraints

- **Free tools only.** Python, Docker, GitHub, GitHub Actions, local DVC remote.
- **Solo execution.** One person across both disciplines and all four modules
  ([ADR-0002](adr/0002-solo-execution.md)).
- **Paced by work unit, not calendar.** A unit is a coherent change that leaves
  the repository working. No document in this repository contains a week-numbered
  schedule.
- **Learning is a first-class objective.** The purpose is to understand every
  technique used, not to acquire working code by the shortest path. Where a
  simpler method would do, the ADR says so and explains why the chosen method was
  worth the cost.
- **Portability over local optimisation.** The development machine has a GPU. The
  Docker image must run without one.

## 9. Non-goals worth naming

- **This is not a machine-learning modelling project.** No credit-risk model is
  built. ML is used for imputation, anomaly detection and column classification —
  as a means to data quality, not as the end.
- **This is not a data-analysis project.** Argus does not answer questions about
  lending. It answers whether the data can be trusted to answer them.
- **Argus does not decide policy.** It reports and scores. Whether a batch at 72%
  health is acceptable is a threshold its operator sets.
