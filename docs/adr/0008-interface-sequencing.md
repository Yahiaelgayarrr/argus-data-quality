# ADR-0008 — Interface build order: CLI, then HTML, then API, then dashboard

- **Date:** 2026-09-27
- **Status:** Accepted

## Context

The brief requires a CLI and treats the system as a backend engine; a dashboard
is out of its scope. This project adds a FastAPI service and a React dashboard,
for two reasons: the compliance/reviewer persona in the charter cannot use a CLI,
and a working dashboard is what makes the final demo legible to a non-technical
audience.

The risk is well known. Frontend work is absorbing and highly visible, and it
tends to consume the time budget of whatever it sits next to. A polished dashboard
over a shallow engine is a worse outcome here than a strong engine with no
dashboard at all — the assessment criteria and the master's-application audience
both reward the engine.

## Decision

Build interfaces in this order, and do not start one before the previous is
working:

**1. CLI — Modules 1 to 3.** The only interface while the engine is being built.
Covers PIPE-006. Interface UX at this stage means: meaningful flags, clear errors
that name the file and field, progress output on long runs, and an exit code that
reflects whether the quality gate passed.

**2. Static HTML report — Modules 1 to 3.** The pipeline writes a self-contained
HTML file with the profiling charts, quality delta and flagged issues embedded. No
server, no build step, no frontend framework. This gives almost all of the
demonstration value of a dashboard for a small fraction of the work, and it is
also the artifact that can be attached to an email or a submission.

**3. FastAPI service — Module 4.** A thin adapter over the Layer 2 API of
ADR-0006: submit a dataset, trigger a run, poll status, retrieve reports. Long
runs execute as background tasks, since a full-dataset run exceeds any sensible
HTTP timeout.

**4. React dashboard — Module 4, last.** Consumes the API. Upload, run history,
profiling explorer, cleaning diff view, validation results with drift and anomaly
views.

**The API contract is designed during the planning phase**, before Module 1 code
exists, and recorded in `docs/06-interfaces.md`. Designing it early costs little
and prevents the Layer 2 API from being shaped around CLI convenience in a way
that has to be unpicked later.

## Stop rule

Steps 3 and 4 are explicitly cuttable. If the engine, its tests, its evaluation
and its documentation are not finished, the dashboard does not get built, and the
HTML report from step 2 carries the demo. This is recorded here so that the
decision to cut is a planned outcome rather than a failure.

## Consequences

**Positive**

- The engine gets the attention, and no module is blocked on frontend work.
- The HTML report is useful from Module 1 onward, not only at the end.
- Because every interface is an adapter, none of this sequencing is retrofitting.

**Negative / accepted trade-offs**

- Two reporting paths exist: static HTML and the API-driven dashboard. They share
  the same report JSON, so the duplication is presentational only.
- The dashboard arrives late and therefore gets the least polish.
- React introduces a Node toolchain to an otherwise pure-Python repository. It is
  isolated in its own directory with its own build, and the Python package never
  depends on it.

## Note on GPU portability

Module 3's autoencoder and the sentence-transformer embeddings use the
development machine's GPU. The Docker image must run on machines without one, so
every GPU-using component requires a CPU fallback path, selected at runtime by
capability detection rather than by configuration. A demo that only runs on the
author's laptop does not satisfy PIPE-007.
