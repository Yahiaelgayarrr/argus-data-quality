# Project Context

**Read this first.** It states where the project actually stands. Everything else
describes what it will be; this file describes what it is.

Last updated: 2026-09-27

---

## Identity

| | |
| --- | --- |
| **Project** | Argus — Automated Data Quality & Validation System |
| **Repository** | `argus-data-quality` |
| **Origin** | CadetX virtual work experience, *Automated Data Cleaning & Validation System* |
| **Execution** | Solo, covering both the AI Engineer and Data Scientist disciplines ([ADR-0002](docs/adr/0002-solo-execution.md)) |
| **Phase** | Module 0 — foundation. Planning documents in progress. |
| **Cadence** | Paced by work unit, not calendar. No week-numbered schedules. |

## Current state — honest version

**No application code exists.** `src/argus/` is empty. There is nothing to run, no
tests to pass, no pipeline, no Docker image, no CI.

What exists is the planning layer: the charter, the requirement traceability
matrix, and nine architecture decision records.

## Status

| Area | Status |
| --- | --- |
| Charter | IMPLEMENTED |
| Requirement traceability | IMPLEMENTED |
| Architecture decision records 0001–0009 | IMPLEMENTED |
| Requirements (functional / non-functional) | PLANNED — next |
| Architecture document | PLANNED |
| Tech stack document | PLANNED |
| Data document | PLANNED |
| Interfaces document (CLI, config, report schemas, API contract) | PLANNED |
| Roadmap | PLANNED |
| Repository scaffold and tooling | PLANNED |
| Dataset acquired locally | NOT STARTED |
| Module 1 — Profiling | NOT STARTED |
| Module 2 — Cleaning | NOT STARTED |
| Module 3 — Validation | NOT STARTED |
| Module 4 — Integration | NOT STARTED |

Requirement-level detail: [`docs/01-brief-traceability.md`](docs/01-brief-traceability.md)
— 82 requirements, 0 verified.

## Decisions locked

| Decision | Record |
| --- | --- |
| Documentation-first; repository is the source of truth | [ADR-0001](docs/adr/0001-documentation-first.md) |
| Solo execution; team mechanics adapted, technical scope unchanged | [ADR-0002](docs/adr/0002-solo-execution.md) |
| Domain: consumer lending. Dataset: Lending Club 2007–2018, 2.26M × 145 | [ADR-0003](docs/adr/0003-domain-and-dataset.md) |
| Hybrid evaluation: real data plus injected labelled corruption | [ADR-0004](docs/adr/0004-hybrid-ground-truth.md) |
| Polars core engine, DuckDB for aggregates, pandas at library boundaries, Parquet storage | [ADR-0005](docs/adr/0005-polars-core-engine.md) |
| Installable package with thin CLI/API adapters; engine/domain-pack split | [ADR-0006](docs/adr/0006-package-first-architecture.md) |
| Rule kinds in Python, rule instances in YAML | [ADR-0007](docs/adr/0007-declarative-rule-config.md) |
| Interface order: CLI → HTML report → FastAPI → React. Last two cuttable. | [ADR-0008](docs/adr/0008-interface-sequencing.md) |
| Data versioning: DVC | [ADR-0009](docs/adr/0009-data-versioning-tool.md) |

**Superseded:** an earlier plan (2026-09-02) selected healthcare and the CMS
Medicare provider dataset. Replaced by ADR-0003, which records why.

## Open questions

| Question | Blocking? |
| --- | --- |
| Verify Lending Club dataset licence terms before ingestion | Blocks committing any DVC pointer |
| Dataset not yet downloaded locally | Blocks Module 1 |
| Whether `emp_title` standardisation uses fuzzy clustering or embedding clustering | No — decided in Module 2 |
| Health-score weighting across quality dimensions | No — decided in Module 3 |

## Verification commands

None yet. These are the intended commands and **do not currently work**:

```bash
pytest                                             # test suite
argus profile --input data.parquet --output out/   # Module 1
argus run --config configs/lending.yaml            # full pipeline
docker run argus:latest --input /data/loans.csv    # containerised
```

## Document map

| Document | Purpose |
| --- | --- |
| [`docs/00-charter.md`](docs/00-charter.md) | What the project is for, who it serves, scope, success criteria |
| [`docs/01-brief-traceability.md`](docs/01-brief-traceability.md) | Every requirement with a stable ID and required evidence |
| [`docs/adr/`](docs/adr/) | Decisions, alternatives considered, trade-offs accepted |
| [`AGENTS.md`](AGENTS.md) | How to work in this repository |
| [`CONTRIBUTING.md`](CONTRIBUTING.md) | Code standards, Git workflow, self-review checklist |

Planned: `02-requirements.md`, `03-architecture.md`, `04-tech-stack.md`,
`05-data.md`, `06-interfaces.md`, `07-testing.md`, `08-deployment.md`,
`09-roadmap.md`, and `docs/modules/`.

## Next action

Write [`docs/02-requirements.md`](docs/02-requirements.md) — functional and
non-functional requirements with acceptance criteria concrete enough to write
tests against, each mapped to a brief requirement ID.
