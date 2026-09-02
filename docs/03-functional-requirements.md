# Functional Requirements

Functional requirements are derived from [docs/00-official-requirements.md](00-official-requirements.md).

## Dataset Loading

| ID | Source | Requirement | Acceptance Criteria |
| --- | --- | --- | --- |
| FR-LOAD-001 | PROJ-001, PROF-001 | Load a tabular dataset from a user-provided path. | Valid CSV loads into the pipeline; invalid path returns a clear error. |
| FR-LOAD-002 | PIPE-003 | Load user configuration from YAML. | Pipeline behavior can be adjusted without code edits. |

## Profiling

| ID | Source | Requirement | Acceptance Criteria |
| --- | --- | --- | --- |
| FR-PROF-001 | PROF-002 | Generate dataset-level metadata. | Report includes rows, columns, memory/size indicators where practical. |
| FR-PROF-002 | PROF-003, PROF-005, PROF-007 | Infer data types and detect type inconsistencies. | Mixed and inconsistent columns are flagged in tests. |
| FR-PROF-003 | PROF-004, PROF-013 | Infer semantic types and potential PII. | Known semantic fixture columns are classified with recorded accuracy. |
| FR-PROF-004 | PROF-006, PROF-008, PROF-009 | Compute missingness, cardinality, distributions, and correlations. | Numeric/categorical fixtures produce expected summary statistics. |
| FR-PROF-005 | PROF-010 | Generate backend visualizations. | Expected visualization files are written for supported data. |
| FR-PROF-006 | PROF-014, PROF-015 | Write `profiling_report.json` matching documented schema. | JSON schema validation passes. |

## Cleaning

| ID | Source | Requirement | Acceptance Criteria |
| --- | --- | --- | --- |
| FR-CLEAN-001 | CLEAN-001 | Infer expected schema for cleaning. | Schema captures expected types, ranges, and formats. |
| FR-CLEAN-002 | CLEAN-002 to CLEAN-006 | Impute missing values using statistical and ML approaches. | Masked-value evaluation records performance. |
| FR-CLEAN-003 | CLEAN-007 | Detect and handle exact/fuzzy duplicates. | Known duplicate pairs are detected with measured precision/recall. |
| FR-CLEAN-004 | CLEAN-008 | Normalize types, strings, dates, and categories. | Dirty fixtures transform into expected normalized values. |
| FR-CLEAN-005 | CLEAN-009 | Apply selected transformations. | Transformations are config-controlled and logged. |
| FR-CLEAN-006 | CLEAN-010 to CLEAN-013 | Produce cleaned data, log, and quality delta. | `cleaned_data.csv` and `cleaning_log.json` are written and tested. |

## Validation

| ID | Source | Requirement | Acceptance Criteria |
| --- | --- | --- | --- |
| FR-VAL-001 | VAL-001 to VAL-003 | Apply deterministic validation rules. | Invalid fixture values are flagged with severity. |
| FR-VAL-002 | VAL-004, VAL-005 | Run Isolation Forest and LOF anomaly detection. | Outputs include anomaly scores and evaluation where ground truth exists. |
| FR-VAL-003 | VAL-007 to VAL-009 | Detect semantic column issues and inconsistencies. | Labeled fixtures provide measurable results. |
| FR-VAL-004 | VAL-010 to VAL-012 | Detect impossible values, suspicious patterns, and drift. | Validation report includes issue categories and evidence. |
| FR-VAL-005 | VAL-013 to VAL-016 | Produce severity, health score, report, and evaluation artifacts. | `validation_report.json` and evaluation files are generated. |

## Integration

| ID | Source | Requirement | Acceptance Criteria |
| --- | --- | --- | --- |
| FR-PIPE-001 | PIPE-001, PIPE-002 | Orchestrate profiling, cleaning, and validation. | End-to-end test writes all expected artifacts. |
| FR-PIPE-002 | PIPE-004 | Log info, warning, and error events. | Pipeline logs meaningful events without leaking sensitive data. |
| FR-PIPE-003 | PIPE-006 | Provide CLI execution. | Command accepts `--input` and `--output`. |
| FR-PIPE-004 | PIPE-007, PIPE-008 | Package with Docker and dependency file. | Docker run reproduces pipeline on sample data. |

