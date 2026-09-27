<h1 align="center">Argus</h1>

<p align="center">
  <strong>Automated Data Quality &amp; Validation System</strong><br>
  <em>Profile · Clean · Validate · Automate</em>
</p>

<p align="center">
  <a href="#status"><img alt="status" src="https://img.shields.io/badge/status-foundation-blue"></a>
  <img alt="python" src="https://img.shields.io/badge/python-3.12-blue">
  <img alt="license" src="https://img.shields.io/badge/license-MIT-lightgrey">
</p>

---

Data arriving at an organisation is rarely fit to use. Missing values that
sometimes mean *unknown* and sometimes mean *not applicable*. The same category
spelled four ways. Numbers stored as text, dates in three formats, duplicates that
are not byte-identical, and columns whose contents do not match their names.

The defects are not exotic. What makes them expensive is that they are invisible at
the point of use: a model trains successfully on corrupted data, a dashboard
renders a confident number from a broken join, and the defect surfaces later as a
wrong decision with a cold trail back to its cause.

**Argus is the gatekeeper that stands in between.** It profiles a dataset, cleans
it while recording every modification, validates it against declared business
rules and machine-learning detectors, and scores its health — so that no dataset
reaches a downstream system unexamined.

Named for the hundred-eyed watchman of Greek myth, set to guard what mattered and
never closing all his eyes at once.

## Status

> **Foundation phase. No application code exists yet.**
>
> This repository currently contains the planning layer: charter, requirement
> traceability, and architecture decision records. There is nothing to run.
> [`CONTEXT.md`](CONTEXT.md) always states the real current state.

Progress is tracked per requirement in
[`docs/01-brief-traceability.md`](docs/01-brief-traceability.md) — 82 requirements,
0 verified.

## What it will do

```
   raw data
      │
      ▼
┌─────────────┐   profiling_report.json
│   PROFILE   │   What is in here? What is broken? What is sensitive?
└─────────────┘   Type inference · semantic detection · missingness ·
      │           cardinality · correlation · PII · suspicious columns
      ▼
┌─────────────┐   cleaned_data.csv + cleaning_log.json
│    CLEAN    │   Fix it — and record every single change
└─────────────┘   Format normalisation · fuzzy duplicates ·
      │           statistical and ML imputation · transformations
      ▼
┌─────────────┐   validation_report.json
│  VALIDATE   │   Can this be trusted?
└─────────────┘   Declarative rules · Isolation Forest · LOF · autoencoder ·
      │           NLP column classifier · drift detection · health score
      ▼
┌─────────────┐
│  AUTOMATE   │   One command. YAML-configured. Dockerised. Tested.
└─────────────┘
```

Two properties matter more than the feature list:

**Every modification is auditable.** `cleaning_log.json` records what changed,
which rows, by which method, and why. A cleaner that improves data without saying
how is not usable in a regulated context.

**The engine is dataset-agnostic.** Domain knowledge lives in a swappable rule
pack, not in the engine. Module 4 verifies this by running the finished pipeline on
an unrelated dataset with configuration changes only.

## Demonstration domain: consumer lending

Built and demonstrated against the Lending Club loan book, 2007–2018 —
**2,260,668 loans across 145 columns** of real production data.

Lending was chosen because data quality there is a regulated concern in its own
right, not merely a prerequisite for good analytics. A defect does not produce a
misleading chart; it produces a mispriced interest rate, a wrongly declined
applicant, or a credit model trained on corrupted history.

The dataset also brings genuine, undesigned mess: `emp_length` holding
`"10+ years"` and `"< 1 year"`, `term` as `" 36 months"`, dates as `"Dec-2015"`,
postcodes masked to `"123xx"`, an entirely null `member_id`, and `emp_title` as
uncontrolled free text written by millions of different people.

Rationale and alternatives considered:
[ADR-0003](docs/adr/0003-domain-and-dataset.md).

## How it will be measured

Claims in this repository are measurements, not assertions.

A **corruption injector** takes a clean slice of the real data, applies defects of
known type and location, and records every one. Detection is then reported as
precision, recall, confusion matrix and ROC against those labels. Imputation
methods are compared by blanking values whose true value is retained, so the
comparison is against ground truth rather than plausibility.

Method and its limitations: [ADR-0004](docs/adr/0004-hybrid-ground-truth.md).

## Architecture

```
Layer 3   CLI    FastAPI    React dashboard    notebooks
             \      |         /                 /
              \     |        /                 /
Layer 2   argus.profile() · clean() · validate() · run_pipeline()
                            |
Layer 1   engine: profiler · cleaner · rule engine · detectors · scoring
```

Layer 2 is the product. Every interface is a thin adapter over it, so no two of
them can drift into disagreeing about what "clean" means.
[ADR-0006](docs/adr/0006-package-first-architecture.md).

## Stack

| Concern | Choice | Why |
| --- | --- | --- |
| Dataframe engine | **Polars** | Lazy, multi-threaded, Arrow-backed. 145 columns × 2.26M rows without sampling. [ADR-0005](docs/adr/0005-polars-core-engine.md) |
| Aggregate profiling | **DuckDB** | SQL over Parquet, out-of-core |
| Storage | **Parquet** | Columnar, typed, compressed |
| ML | **scikit-learn**, **PyTorch** | Imputers and detectors; autoencoder on GPU with CPU fallback |
| Semantic column typing | **sentence-transformers** | Column-meaning classification |
| Config | **YAML** + schema validation | Rule kinds in Python, rule instances as data. [ADR-0007](docs/adr/0007-declarative-rule-config.md) |
| Data versioning | **DVC** | Stage graph, so every reported number traces to input hashes. [ADR-0009](docs/adr/0009-data-versioning-tool.md) |
| Quality | **pytest**, **ruff**, **black**, **pre-commit** | |
| Delivery | **Docker**, **GitHub Actions** | |

## Documentation

| | |
| --- | --- |
| [`CONTEXT.md`](CONTEXT.md) | **Start here.** Real current state, locked decisions, next action |
| [`docs/00-charter.md`](docs/00-charter.md) | Problem, users, scope, success criteria |
| [`docs/01-brief-traceability.md`](docs/01-brief-traceability.md) | All 82 requirements with required evidence |
| [`docs/adr/`](docs/adr/) | Why the system is built this way |
| [`AGENTS.md`](AGENTS.md) | How to work in this repository |
| [`CONTRIBUTING.md`](CONTRIBUTING.md) | Standards, Git workflow, self-review checklist |

## Provenance

Built for the CadetX virtual work experience programme, *Automated Data Cleaning &
Validation System*. Executed solo rather than as the programme's team of three,
covering both the AI Engineer and Data Scientist disciplines across all four
modules. Team-coordination mechanics are adapted to solo equivalents; the technical
scope is unchanged, and the adaptation is documented rather than glossed over —
[ADR-0002](docs/adr/0002-solo-execution.md).

Author: Yahia El Gayar

## Licence

MIT — see [`LICENSE`](LICENSE).

The Lending Club dataset is **not** redistributed here and is not committed to this
repository in any form. Obtain it from its original source and observe its terms.
