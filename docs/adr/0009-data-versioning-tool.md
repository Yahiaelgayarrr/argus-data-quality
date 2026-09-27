# ADR-0009 — Data versioning with DVC

- **Date:** 2026-09-27
- **Status:** Accepted
- **Resolves:** DECISION-002 (previously open)

## Context

PIPE-005 requires data versioning using DVC or Git LFS. The datasets involved are
large: the compressed accepted-loans file is roughly 1.6 GB, and the pipeline
generates further large artifacts — the Parquet conversion, corrupted variants,
and cleaned outputs.

Two distinct needs are in play. One is storing large files outside Git. The other
is being able to state, for any reported metric, exactly which input produced it.

## Options considered

**Git LFS.** Stores large files in a side-channel while keeping Git's interface.
Simple, and well supported by GitHub. But it versions *files* only. It has no
concept of a pipeline stage, no record that this output came from that input via
that command, and GitHub's free LFS quota is small relative to these file sizes.

**DVC.** Stores large files in a remote and tracks small metafiles in Git, like
LFS, but adds a pipeline model: stages declare dependencies, outputs and the
command that connects them. `dvc.lock` records the content hash of every input and
output of every stage. Reproducing a reported result becomes a checkout plus a
`dvc repro`, rather than an attempt to remember which version of the data was
loaded that day.

## Decision

Use **DVC**.

The deciding factor is not file storage — either tool solves that. It is that
ADR-0004 makes this project's central claims *measurements*: this imputer scored
that RMSE, this detector achieved that recall. A measurement whose inputs cannot
be identified is an anecdote. DVC's stage graph makes the chain from raw file to
reported number explicit and checkable, which is the same traceability property
that motivated the lending domain in ADR-0003.

**Configuration**

- Raw data is immutable. Once ingested it is never modified in place.
- A local DVC remote is used during development; no paid storage is required.
- `data/raw/`, `data/interim/` and `data/processed/` are Git-ignored; only DVC
  metafiles are committed.
- Pipeline stages are declared in `dvc.yaml`: ingest, sample, corrupt, profile,
  clean, validate.
- `dvc.lock` is committed, so every reported result traces to exact input hashes.

**The dataset is never committed to Git**, in any form, under any circumstances.
It exceeds GitHub's file limits, and a large blob committed once stays in history
permanently even after deletion.

## Consequences

**Positive**

- Reported metrics are reproducible from a recorded state.
- Stage caching means changing a validation threshold does not re-run ingestion.
- `dvc.yaml` doubles as documentation of the pipeline's real shape.

**Negative / accepted trade-offs**

- DVC is a second version-control tool to learn, with its own commands and
  failure modes, layered on Git.
- A cloned repository is not immediately runnable: the dataset must be fetched or
  re-downloaded. The README must say so plainly, and a small committed fixture
  dataset ensures the test suite runs without any large download.
- With a local-only remote, a fresh clone on another machine cannot pull the data.
  Acceptable for a single-developer project; documented as a limitation.
