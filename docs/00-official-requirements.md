# Official Requirements

Source: CadetX Virtual Work Experience brief, "Automated Data Cleaning & Validation System."

This document preserves the official requirements in traceable form. Status values must not be upgraded to IMPLEMENTED or VERIFIED without repository evidence.

## Project-Level Requirements

| ID | Type | Requirement | Status |
| --- | --- | --- | --- |
| PROJ-001 | REQUIRED | Build an Automated Data Quality & Validation System backend engine. | PLANNED |
| PROJ-002 | REQUIRED | The system must profile, clean, validate, and automate dataset checks. | PLANNED |
| PROJ-003 | REQUIRED | The system should run end-to-end with minimal human intervention. | PLANNED |
| PROJ-004 | REQUIRED | Complete four build modules across approximately 3 months. | PLANNED |
| PROJ-005 | REQUIRED | Use free tools, including Python, Docker, GitHub, and optionally GitHub Actions. | PLANNED |
| PROJ-006 | REQUIRED | Submit the GitHub repository link through the CadetX portal. | NOT STARTED |
| PROJ-007 | AMBIGUOUS | Official process assumes a team of three; this repository is planned for solo work if CadetX approves. | OPEN |

## Module 1 - Profiling

| ID | Type | Requirement | Expected Evidence | Status |
| --- | --- | --- | --- | --- |
| PROF-001 | REQUIRED | Accept a dataset as input. | Profiling API/CLI tests | PLANNED |
| PROF-002 | REQUIRED | Extract dataset metadata. | `profiling_report.json` | PLANNED |
| PROF-003 | REQUIRED | Infer column data types. | Report schema and tests | PLANNED |
| PROF-004 | REQUIRED | Detect semantic meaning such as name, email, date, ID. | Report and evaluation | PLANNED |
| PROF-005 | REQUIRED | Detect mixed-type columns. | Report and tests | PLANNED |
| PROF-006 | REQUIRED | Analyze missing values. | Missing-value matrix/report | PLANNED |
| PROF-007 | REQUIRED | Check data-type consistency. | Report and tests | PLANNED |
| PROF-008 | REQUIRED | Analyze unique values and cardinality. | Report | PLANNED |
| PROF-009 | REQUIRED | Produce correlation matrix and distributions where applicable. | Report/visual outputs | PLANNED |
| PROF-010 | REQUIRED | Generate backend profiling visualizations: missing heatmap, outlier distribution, correlation heatmap. | Output artifacts | PLANNED |
| PROF-011 | REQUIRED | Detect suspicious columns. | Report | PLANNED |
| PROF-012 | REQUIRED | Detect inconsistent formats. | Report | PLANNED |
| PROF-013 | REQUIRED | Detect potential PII columns. | Report | PLANNED |
| PROF-014 | REQUIRED | Output `profiling_report.json`. | File artifact | PLANNED |
| PROF-015 | REQUIRED | Document JSON schema, function signatures, and integration instructions. | Markdown docs | PLANNED |

## Module 2 - Cleaning

| ID | Type | Requirement | Expected Evidence | Status |
| --- | --- | --- | --- | --- |
| CLEAN-001 | REQUIRED | Infer expected schema: types, ranges, and formats. | Cleaning log/tests | PLANNED |
| CLEAN-002 | REQUIRED | Handle missing values. | Tests and cleaning log | PLANNED |
| CLEAN-003 | REQUIRED | Implement statistical imputation. | Tests/evaluation | PLANNED |
| CLEAN-004 | REQUIRED | Implement ML imputation: KNN imputer. | Tests/evaluation | PLANNED |
| CLEAN-005 | REQUIRED | Implement ML imputation: regression imputer. | Tests/evaluation | PLANNED |
| CLEAN-006 | REQUIRED | Implement ML imputation: iterative imputer. | Tests/evaluation | PLANNED |
| CLEAN-007 | REQUIRED | Detect duplicates using fuzzy matching. | Tests/evaluation | PLANNED |
| CLEAN-008 | REQUIRED | Normalize types, strings, dates, and categorical values. | Tests/log | PLANNED |
| CLEAN-009 | REQUIRED | Provide transformations: scaling, encoding, normalization, feature extraction. | Tests/log | PLANNED |
| CLEAN-010 | REQUIRED | Score data quality before cleaning. | Cleaning log | PLANNED |
| CLEAN-011 | REQUIRED | Score data quality after cleaning and produce quality delta. | Cleaning log | PLANNED |
| CLEAN-012 | REQUIRED | Output `cleaned_data.csv`. | File artifact | PLANNED |
| CLEAN-013 | REQUIRED | Output `cleaning_log.json`. | File artifact | PLANNED |
| CLEAN-014 | REQUIRED | Provide pytest unit and integration tests. | Test suite | PLANNED |

## Module 3 - Validation

| ID | Type | Requirement | Expected Evidence | Status |
| --- | --- | --- | --- | --- |
| VAL-001 | REQUIRED | Validate email, phone, and postcode values. | Tests/report | PLANNED |
| VAL-002 | REQUIRED | Validate numeric ranges. | Tests/report | PLANNED |
| VAL-003 | REQUIRED | Validate categorical consistency. | Tests/report | PLANNED |
| VAL-004 | REQUIRED | Implement Isolation Forest anomaly detection. | Evaluation/report | PLANNED |
| VAL-005 | REQUIRED | Implement Local Outlier Factor anomaly detection. | Evaluation/report | PLANNED |
| VAL-006 | OPTIONAL | Implement autoencoder anomaly detection as an advanced technique if justified. | Experiment/results | OPEN |
| VAL-007 | REQUIRED | Classify column meaning using NLP or semantic methods. | Evaluation/report | PLANNED |
| VAL-008 | REQUIRED | Detect mislabeled columns. | Evaluation/report | PLANNED |
| VAL-009 | REQUIRED | Detect semantic inconsistencies. | Evaluation/report | PLANNED |
| VAL-010 | REQUIRED | Detect impossible values. | Tests/report | PLANNED |
| VAL-011 | REQUIRED | Detect suspicious patterns. | Tests/report | PLANNED |
| VAL-012 | REQUIRED | Detect data drift. | Evaluation/report | PLANNED |
| VAL-013 | REQUIRED | Score error severity and anomaly severity. | Validation report | PLANNED |
| VAL-014 | REQUIRED | Produce overall dataset health score. | Validation report | PLANNED |
| VAL-015 | REQUIRED | Output `validation_report.json`. | File artifact | PLANNED |
| VAL-016 | REQUIRED | Evaluate validation with precision/recall, confusion matrix, and ROC curves where methodologically appropriate. | Evaluation artifacts | PLANNED |

## Module 4 - Integration

| ID | Type | Requirement | Expected Evidence | Status |
| --- | --- | --- | --- | --- |
| PIPE-001 | REQUIRED | Define input/output schemas. | Architecture docs/tests | PLANNED |
| PIPE-002 | REQUIRED | Orchestrate profiling, cleaning, and validation. | Integration tests | PLANNED |
| PIPE-003 | REQUIRED | Provide YAML configuration for pipeline settings. | Config examples/docs | PLANNED |
| PIPE-004 | REQUIRED | Provide logging with info, warning, and error logs. | Logs/tests | PLANNED |
| PIPE-005 | REQUIRED | Include data versioning using DVC or Git LFS. | Config/docs | OPEN |
| PIPE-006 | REQUIRED | Provide CLI command similar to `python pipeline.py --input data.csv --output results/`. | CLI tests/docs | PLANNED |
| PIPE-007 | REQUIRED | Provide Docker containerization. | Dockerfile/docs | PLANNED |
| PIPE-008 | REQUIRED | Provide `requirements.txt`. | Dependency file | PLANNED |
| PIPE-009 | REQUIRED | Provide automated tests with pytest. | Test suite | PLANNED |
| PIPE-010 | REQUIRED | Provide integration tests. | Test suite | PLANNED |
| PIPE-011 | REQUIRED | Document architecture, API usage, and pipeline flow. | Docs | PLANNED |
| PIPE-012 | REQUIRED | Prepare final demo with architecture diagram, working demo, and results. | Presentation artifacts | PLANNED |

## Process, GitHub, and Presentation Requirements

| ID | Type | Requirement | Solo Adaptation | Status |
| --- | --- | --- | --- | --- |
| PROC-001 | REQUIRED | Choose a domain and collect a dataset before building. | Solo decision gate with documented rationale. | OPEN |
| PROC-002 | REQUIRED | Hold planning meetings. | Weekly solo planning notes or milestone reviews. | PLANNED |
| PROC-003 | REQUIRED | Set up GitHub repository. | Repository exists locally; remote submission pending. | IN PROGRESS |
| PROC-004 | REQUIRED | Keep work updated in GitHub. | Regular commits after explicit approval. | NOT STARTED |
| PROC-005 | REQUIRED | Use branches, PRs, reviews, and merges. | Use feature branches and self-review or mentor review where possible. | PLANNED |
| PROC-006 | REQUIRED | Each module has presentation or documentation. | Per-module docs and final presentation. | PLANNED |
| PROC-007 | REQUIRED | Final presentation to mentor. | Solo final presentation. | NOT STARTED |
| PROC-008 | REQUIRED | Demonstrate consistency and weekly progress. | Track progress in status docs/changelog/Git. | PLANNED |

## Evaluation Criteria

CadetX assessment is based on:

- Consistency
- Engineering quality
- Collaboration evidence
- Completeness
- Documentation
- Final demo

For solo work, collaboration evidence must be adapted into visible GitHub discipline, issue/branch hygiene, self-review notes, and clear weekly progress.

