# 12-Week Roadmap

The roadmap is a planning guide, not a promise. Update it honestly as reality changes.

## Week 1 - Foundation and Decisions

- Goals: finalize planning docs, approve dataset strategy, approve base stack.
- Learning: data-quality workflow, requirements traceability, ADRs.
- Artifacts: Phase 0 docs, decision updates.
- Exit criteria: dataset/domain investigation can begin.

## Week 2 - Dataset and Evaluation Foundation

- Goals: select dataset, define corruption strategy, create fixtures.
- Learning: missingness mechanisms, ground truth, leakage.
- Tests: fixture loading and corruption label checks.
- Exit criteria: reproducible sample/evaluation data exists.

## Week 3 - Profiling Core

- Goals: metadata, type inference, missingness, cardinality.
- Learning: profiling statistics, cardinality, mixed types.
- Tests: unit tests for core profilers.
- Exit criteria: draft `profiling_report.json` schema and core outputs.

## Week 4 - Profiling Intelligence and Visuals

- Goals: semantic inference, PII, suspicious columns, visualizations.
- Learning: semantic classification, correlations, visualization QA.
- Tests: semantic fixture and visualization smoke tests.
- Exit criteria: Module 1 can run on demo dataset.

## Week 5 - Cleaning Core

- Goals: schema inference, normalization, statistical imputation.
- Learning: imputation basics, data leakage, transformations.
- Tests: unit tests for normalization and simple imputation.
- Exit criteria: cleaning log records deterministic actions.

## Week 6 - Cleaning ML and Duplicates

- Goals: KNN/regression/iterative imputation and fuzzy duplicate handling.
- Learning: KNN imputation, iterative imputation, fuzzy matching.
- Tests: imputation and duplicate evaluation.
- Exit criteria: Module 2 produces cleaned CSV and quality delta.

## Week 7 - Validation Rules

- Goals: deterministic validators for email, phone, postcode, numeric, categorical rules.
- Learning: validation contracts and severity scoring.
- Tests: invalid-value fixtures.
- Exit criteria: rule validation writes report sections.

## Week 8 - AI Validation and Drift

- Goals: Isolation Forest, LOF, semantic inconsistency, drift checks.
- Learning: anomaly detection, precision/recall, ROC/AUC.
- Tests: labeled anomaly/semantic fixtures.
- Exit criteria: Module 3 produces evaluated validation report.

## Week 9 - Integration and CLI

- Goals: orchestrator, config loading, CLI, artifact layout.
- Learning: package design, CLI ergonomics, logging.
- Tests: module integration and end-to-end CLI test.
- Exit criteria: full local pipeline run works.

## Week 10 - Docker, CI, and Data Versioning

- Goals: Dockerfile, requirements, data versioning choice, optional GitHub Actions.
- Learning: Docker, reproducibility, CI.
- Tests: Docker smoke test and CI command parity.
- Exit criteria: reproducible environment works.

## Week 11 - Evaluation, Hardening, and Documentation

- Goals: complete evaluation artifacts, fix edge cases, update docs.
- Learning: error analysis, limitations, portfolio explanation.
- Tests: full test suite and regression coverage.
- Exit criteria: demo results are credible and documented.

## Week 12 - Final Demo and Portfolio Polish

- Goals: final presentation, README, module summaries, demo rehearsal.
- Learning: technical storytelling and defense.
- Tests: final clean run from setup instructions.
- Exit criteria: repository is ready to submit and present.

