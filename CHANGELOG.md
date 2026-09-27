# Changelog

Notable changes per work unit. This project is paced by work unit rather than by
calendar ([ADR-0002](docs/adr/0002-solo-execution.md)), so entries are grouped by
change, not by date or sprint.

Format loosely follows [Keep a Changelog](https://keepachangelog.com/).

---

## Unreleased

### Added — foundation

- **Project charter** ([`docs/00-charter.md`](docs/00-charter.md)): problem
  statement, the lending domain rationale, three user personas that design
  decisions are settled against, explicit in-scope and out-of-scope lists, 13
  checkable success criteria, and named non-goals.
- **Requirement traceability** ([`docs/01-brief-traceability.md`](docs/01-brief-traceability.md)):
  the CadetX brief decomposed into 82 individually tracked requirements with stable
  IDs and a required-evidence column for each, plus a separate `EXTENDED` section
  for this project's additions beyond the brief.
- **Architecture decision records** ([`docs/adr/`](docs/adr/)) 0001–0010, each
  recording the alternatives considered and the trade-off accepted.
- **Operating manual** ([`AGENTS.md`](AGENTS.md)): required reading order, status
  vocabulary, and the rules that keep documentation honest.
- **Contribution standards** ([`CONTRIBUTING.md`](CONTRIBUTING.md)): Git workflow,
  commit and PR format, and a self-review checklist that is the only review this
  code will get.

### Decided

- Project named **Argus**. Repository renamed to `argus-data-quality`.
- **Domain: consumer lending.** Dataset: Lending Club accepted loans 2007–2018,
  2,260,668 rows × 145 columns ([ADR-0003](docs/adr/0003-domain-and-dataset.md)).
- **Evaluation is hybrid**: real data as the substrate, with a corruption injector
  producing labelled ground truth so that Module 3's metrics are measurements
  rather than assertions ([ADR-0004](docs/adr/0004-hybrid-ground-truth.md)).
- **Polars** as the core dataframe engine, DuckDB for aggregate profiling, pandas
  only at scikit-learn's boundary, Parquet as storage
  ([ADR-0005](docs/adr/0005-polars-core-engine.md)).
- **Package-first architecture** with thin CLI and API adapters, and a strict
  engine/domain-pack separation ([ADR-0006](docs/adr/0006-package-first-architecture.md)).
- **Rule kinds in Python, rule instances in YAML**
  ([ADR-0007](docs/adr/0007-declarative-rule-config.md)).
- **Interface build order** CLI → HTML report → FastAPI → React, with the last two
  explicitly cuttable ([ADR-0008](docs/adr/0008-interface-sequencing.md)).
- **Autoencoder anomaly detection moved into scope.** The brief marks it optional;
  available GPU hardware removes the reason to defer it, and it is paired with a
  mandatory CPU fallback so the Docker image stays portable
  ([ADR-0010](docs/adr/0010-autoencoder-in-scope.md), superseding the earlier
  deferral).
- **Data versioning: DVC** over Git LFS, chosen for its stage graph rather than for
  file storage ([ADR-0009](docs/adr/0009-data-versioning-tool.md)).

### Fixed

- **`.gitignore` no longer depends on where a dataset is downloaded to.** Data
  extensions were only ignored under `data/`, so a 1.6 GB loan file saved to the
  repository root was stageable. They are now ignored repository-wide, with
  explicit negations for the small committed fixtures under `tests/fixtures/` and
  `data/fixtures/`. A large blob committed once stays in Git history permanently,
  so this had to be closed before the dataset arrives.
- **PIPE-006 now records how the brief's exact command is satisfied.** The plan
  named only `argus run`; the brief specifies `python pipeline.py --input …`. A
  root-level `pipeline.py` shim over the `argus` entry point is now the stated
  approach, so the requirement is met verbatim rather than by rebranding.
- **VAL-006 cited ADR-0008**, which is about interface build order and says
  nothing about the autoencoder. It now cites ADR-0010.

### Superseded

- An earlier plan (2026-09-02) selected the healthcare domain and the CMS
  *Medicare Physician & Other Practitioners* dataset. Superseded by ADR-0003: CMS
  data is aggregated to provider-service level, which removes most row-level
  defects and leaves PII detection and the NLP column classifier with almost
  nothing real to work on. The original decision is preserved in ADR-0003 rather
  than deleted.
- `DECISION-002` (data versioning, previously open) resolved by ADR-0009.
- `DECISION-004` (autoencoder deferred) superseded by ADR-0010, which brings it
  into scope and states what would make it worth keeping.

### Removed

- The previous planning scaffold — 17 documents that were placeholder tables,
  generic option matrices, or contradicted the work-unit cadence (notably a
  12-week schedule). Recoverable from Git history. `AGENTS.md` was the one file
  worth carrying forward, and it was rewritten around the decisions above.

### Not yet started

No application code exists. `src/argus/` is empty; there is nothing to run and no
test suite. [`CONTEXT.md`](CONTEXT.md) is the authoritative statement of current
state.
