# Requirement Traceability

Every requirement in the CadetX brief for *Automated Data Cleaning & Validation
System*, decomposed into individually trackable items with stable IDs.

**Why IDs.** Code, tests, commits, module documents and ADRs all reference the same
identifier, so it is always answerable whether a given requirement is done and
what proves it. At submission, this table is the completeness argument.

**Status discipline.** Type and status vocabulary are defined in
[`AGENTS.md`](../AGENTS.md). A row may only reach `VERIFIED` when a passing test or
a recorded result proves it. Nothing here may be upgraded optimistically.

Everything is `PLANNED` at present. No implementation exists.

---

## Project level

| ID | Type | Requirement | Status |
| --- | --- | --- | --- |
| PROJ-001 | REQUIRED | Build a backend data-quality engine that acts as a gatekeeper for datasets entering the organisation | PLANNED |
| PROJ-002 | REQUIRED | The system profiles, cleans, validates and automates dataset checks | PLANNED |
| PROJ-003 | REQUIRED | The system runs end-to-end with minimal human intervention | PLANNED |
| PROJ-004 | REQUIRED | Complete all four build modules | PLANNED |
| PROJ-005 | REQUIRED | Use free tools only — Python, Docker, GitHub, GitHub Actions | PLANNED |
| PROJ-006 | REQUIRED | Submit the GitHub repository link via the CadetX portal | NOT STARTED |
| PROJ-007 | AMBIGUOUS | The brief assumes a team of three. Executed solo; participation confirmed. Resolved by [ADR-0002](adr/0002-solo-execution.md) | RESOLVED |

## Module 1 — Advanced Data Profiling & Metadata Intelligence

Output: `profiling_report.json`

| ID | Type | Requirement | Evidence required | Status |
| --- | --- | --- | --- | --- |
| PROF-001 | REQUIRED | Accept a dataset as input | API and CLI tests | PLANNED |
| PROF-002 | REQUIRED | Extract dataset-level metadata | `profiling_report.json` | PLANNED |
| PROF-003 | REQUIRED | Infer column data types | Report schema plus tests | PLANNED |
| PROF-004 | REQUIRED | Detect semantic meaning (name, email, date, ID, money, postcode, free text) | Report plus labelled-fixture accuracy | PLANNED |
| PROF-005 | REQUIRED | Detect mixed-type columns | Report plus tests | PLANNED |
| PROF-006 | REQUIRED | Analyse missing values, including co-missingness structure | Missing-value matrix | PLANNED |
| PROF-007 | REQUIRED | Check data-type consistency | Report plus tests | PLANNED |
| PROF-008 | REQUIRED | Analyse unique values and cardinality | Report | PLANNED |
| PROF-009 | REQUIRED | Produce correlation matrix and distributions | Report plus visual artifacts | PLANNED |
| PROF-010 | REQUIRED | Generate backend visualisations: missing heatmap, outlier distribution, correlation heatmap | Image artifacts | PLANNED |
| PROF-011 | REQUIRED | Detect suspicious columns (all-null, constant, ID-like, URL) | Report | PLANNED |
| PROF-012 | REQUIRED | Detect inconsistent formats | Report | PLANNED |
| PROF-013 | REQUIRED | Detect potential PII columns, including quasi-identifiers | Report | PLANNED |
| PROF-014 | REQUIRED | Write `profiling_report.json` | File artifact plus schema validation | PLANNED |
| PROF-015 | REQUIRED | Document the JSON schema, function signatures and integration instructions | `docs/modules/module-1-profiling.md` | PLANNED |

## Module 2 — Advanced Data Cleaning & Transformation Pipeline

Outputs: `cleaned_data.csv`, `cleaning_log.json`

| ID | Type | Requirement | Evidence required | Status |
| --- | --- | --- | --- | --- |
| CLEAN-001 | REQUIRED | Infer expected schema: types, ranges, formats | Schema output plus tests | PLANNED |
| CLEAN-002 | REQUIRED | Handle missing values | Tests plus cleaning log | PLANNED |
| CLEAN-003 | REQUIRED | Statistical imputation (median, mode) | Masked-value evaluation | PLANNED |
| CLEAN-004 | REQUIRED | KNN imputation | Masked-value evaluation | PLANNED |
| CLEAN-005 | REQUIRED | Regression imputation | Masked-value evaluation | PLANNED |
| CLEAN-006 | REQUIRED | Iterative imputation | Masked-value evaluation | PLANNED |
| CLEAN-007 | REQUIRED | Duplicate detection including fuzzy matching | Precision/recall on labelled pairs | PLANNED |
| CLEAN-008 | REQUIRED | Normalise types, strings, dates and categorical values | Dirty-fixture tests | PLANNED |
| CLEAN-009 | REQUIRED | Transformations: scaling, encoding, normalisation, feature extraction | Config-driven, logged, tested | PLANNED |
| CLEAN-010 | REQUIRED | Score data quality before cleaning | Quality report | PLANNED |
| CLEAN-011 | REQUIRED | Score after cleaning and produce the delta | Quality report | PLANNED |
| CLEAN-012 | REQUIRED | Write `cleaned_data.csv` | File artifact | PLANNED |
| CLEAN-013 | REQUIRED | Write `cleaning_log.json` | File artifact | PLANNED |
| CLEAN-014 | REQUIRED | pytest unit and integration tests | Test suite | PLANNED |

## Module 3 — AI Validation, Anomaly Detection & Rule Engine

Output: `validation_report.json`

| ID | Type | Requirement | Evidence required | Status |
| --- | --- | --- | --- | --- |
| VAL-001 | REQUIRED | Validate email, phone and postcode values | Tests plus report | PLANNED |
| VAL-002 | REQUIRED | Validate numeric ranges | Tests plus report | PLANNED |
| VAL-003 | REQUIRED | Validate categorical consistency | Tests plus report | PLANNED |
| VAL-004 | REQUIRED | Isolation Forest anomaly detection | Evaluation against injected labels | PLANNED |
| VAL-005 | REQUIRED | Local Outlier Factor anomaly detection | Evaluation against injected labels | PLANNED |
| VAL-006 | REQUIRED | Autoencoder anomaly detection. The brief marks this advanced/optional; this project commits to it — see [ADR-0010](adr/0010-autoencoder-in-scope.md) | Evaluation against the same labels as VAL-004/005, plus a CPU-fallback test | PLANNED |
| VAL-007 | REQUIRED | Classify column meaning using NLP / semantic methods | Labelled-column accuracy | PLANNED |
| VAL-008 | REQUIRED | Detect mislabelled columns | Detection rate on injected swaps | PLANNED |
| VAL-009 | REQUIRED | Detect semantic inconsistencies | Evaluation plus report | PLANNED |
| VAL-010 | REQUIRED | Detect impossible values | Tests plus report | PLANNED |
| VAL-011 | REQUIRED | Detect suspicious patterns | Tests plus report | PLANNED |
| VAL-012 | REQUIRED | Detect data drift | PSI / KS / chi-square across issue years | PLANNED |
| VAL-013 | REQUIRED | Score error severity and anomaly severity | `validation_report.json` | PLANNED |
| VAL-014 | REQUIRED | Produce an overall dataset health score | `validation_report.json` | PLANNED |
| VAL-015 | REQUIRED | Write `validation_report.json` | File artifact | PLANNED |
| VAL-016 | REQUIRED | Evaluate with precision, recall, confusion matrix and ROC curves | Evaluation artifacts plus plots | PLANNED |

## Module 4 — Integration, Orchestration & Production Engineering

Output: end-to-end pipeline (CLI + Docker)

| ID | Type | Requirement | Evidence required | Status |
| --- | --- | --- | --- | --- |
| PIPE-001 | REQUIRED | Define input/output schemas between stages | Schema definitions plus tests | PLANNED |
| PIPE-002 | REQUIRED | Orchestrate profiling → cleaning → validation | Integration test | PLANNED |
| PIPE-003 | REQUIRED | YAML configuration for pipeline settings | Config files plus schema validation | PLANNED |
| PIPE-004 | REQUIRED | Logging at info, warning and error levels | Log output plus tests | PLANNED |
| PIPE-005 | REQUIRED | Data versioning. Resolved as DVC by [ADR-0009](adr/0009-data-versioning-tool.md) | `dvc.yaml`, `dvc.lock` | PLANNED |
| PIPE-006 | REQUIRED | CLI: `python pipeline.py --input data.csv --output results/`. Satisfied by an `argus` console entry point, plus a root `pipeline.py` shim delegating to it so the brief's exact command works verbatim | CLI tests for both invocations, plus docs | PLANNED |
| PIPE-007 | REQUIRED | Docker containerisation, running without a GPU | `Dockerfile` plus a verified run | PLANNED |
| PIPE-008 | REQUIRED | Pinned dependency manifest | `requirements.txt` / `pyproject.toml` | PLANNED |
| PIPE-009 | REQUIRED | Automated tests with pytest | Test suite | PLANNED |
| PIPE-010 | REQUIRED | Integration tests | Test suite | PLANNED |
| PIPE-011 | REQUIRED | Document architecture, API usage and pipeline flow | `docs/03-architecture.md`, `docs/06-interfaces.md` | PLANNED |
| PIPE-012 | REQUIRED | Final demo: architecture diagram, working demo, results | Presentation artifacts | PLANNED |

## Process and submission

Team mechanics are adapted per [ADR-0002](adr/0002-solo-execution.md).

| ID | Type | Requirement | How it is satisfied here | Status |
| --- | --- | --- | --- | --- |
| PROC-001 | REQUIRED | Choose a domain and collect a dataset before building | Consumer lending / Lending Club, [ADR-0003](adr/0003-domain-and-dataset.md) | RESOLVED |
| PROC-002 | REQUIRED | Hold planning meetings | Written planning documents and ADRs in place of meetings | IN PROGRESS |
| PROC-003 | REQUIRED | Set up the GitHub repository | `argus-data-quality` | IMPLEMENTED |
| PROC-004 | REQUIRED | Keep work updated in GitHub | Commits per work unit | IN PROGRESS |
| PROC-005 | REQUIRED | Use branches, pull requests, reviews and merges | Branch per work unit, PR with description, documented self-review | IN PROGRESS |
| PROC-006 | REQUIRED | Each module has its own presentation or documentation | One document per module in `docs/modules/` | PLANNED |
| PROC-007 | REQUIRED | Final presentation to mentor | Solo final presentation | NOT STARTED |
| PROC-008 | REQUIRED | Demonstrate consistency and ongoing progress | Commit history, PR history and `CHANGELOG.md` | IN PROGRESS |

**Note on PROC-005 and the brief's `Collaboration` criterion.** With one person,
pull requests are self-reviewed. This is weaker than independent review and is
stated plainly rather than presented as equivalent. What the Git history does
evidence is discipline: scoped branches, written PR rationale, and a repository
whose default branch always works.

## Extended requirements

Additions beyond the brief, tracked separately so a reviewer can see exactly what
is spec and what is this project's own choice. These are `EXTENDED`: they must
never be traded against a `REQUIRED` row.

| ID | Type | Requirement | Rationale | Status |
| --- | --- | --- | --- | --- |
| EXT-001 | EXTENDED | Corruption injector producing labelled ground truth | Without labels, VAL-016 cannot be measured. See [ADR-0004](adr/0004-hybrid-ground-truth.md) | PLANNED |
| EXT-002 | EXTENDED | Engine/domain-pack separation with no dataset-specific names in the engine | Makes "runs on any dataset" demonstrable. See [ADR-0006](adr/0006-package-first-architecture.md) | PLANNED |
| EXT-003 | EXTENDED | Generalisation run on a second, unrelated dataset with config changes only | Proves EXT-002 | PLANNED |
| EXT-004 | EXTENDED | Self-contained HTML report | The compliance persona cannot use a CLI. See [ADR-0008](adr/0008-interface-sequencing.md) | PLANNED |
| EXT-005 | EXTENDED | FastAPI service over the public API | Cuttable | PLANNED |
| EXT-006 | EXTENDED | React dashboard | Cuttable; last in build order | PLANNED |
| EXT-007 | EXTENDED | Imputation strategies compared on held-out true values | "ML imputation" is required; choosing between methods on evidence is not | PLANNED |
| EXT-008 | EXTENDED | Leakage flagging of post-origination columns | Domain insight: `total_pymnt`, `recoveries` etc. must not reach a credit model | PLANNED |
| EXT-009 | EXTENDED | Full-scale run on 2.26M rows rather than a sample | Makes the engineering choices in [ADR-0005](adr/0005-polars-core-engine.md) real | PLANNED |
| EXT-010 | EXTENDED | ADR discipline for every significant decision | Learning objective; see `docs/00-charter.md` §8 | IN PROGRESS |

## Coverage summary

| Group | Count | Verified |
| --- | --- | --- |
| Project | 7 | 0 |
| Module 1 — Profiling | 15 | 0 |
| Module 2 — Cleaning | 14 | 0 |
| Module 3 — Validation | 16 | 0 |
| Module 4 — Integration | 12 | 0 |
| Process | 8 | — |
| Extended | 10 | 0 |
| **Total** | **82** | **0** |

Update the verified count when rows change status. A requirement is only complete
when its evidence column is satisfied.
