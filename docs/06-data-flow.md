# Data Flow

Status: proposed, not implemented.

## End-to-End Flow

```text
input
-> loading
-> profiling
-> cleaning
-> validation
-> scoring
-> reports
-> final output
```

## Stage 1 - Input

Inputs:

- User-provided dataset, initially CSV.
- YAML configuration.
- Optional ground-truth labels for corrupted/evaluation datasets.

Artifacts:

- None required before loading.

## Stage 2 - Loading

Responsibilities:

- Read dataset.
- Validate readable format.
- Preserve raw input unchanged.
- Create internal dataset representation.

Artifacts:

- Future: optional load summary in pipeline metadata.

## Stage 3 - Profiling

Responsibilities:

- Metadata extraction.
- Type and semantic inference.
- Missingness, cardinality, distribution, and correlation analysis.
- Suspicious column and potential PII detection.
- Backend visualizations.

Artifacts:

- `profiling_report.json`
- Visualization files, if enabled

## Stage 4 - Cleaning

Responsibilities:

- Apply schema-informed cleaning.
- Handle missing values.
- Normalize values and types.
- Detect and handle duplicates.
- Apply selected transformations.
- Score quality before and after cleaning.

Artifacts:

- `cleaned_data.csv`
- `cleaning_log.json`

## Stage 5 - Validation

Responsibilities:

- Apply deterministic rules.
- Run anomaly detection.
- Run semantic consistency checks.
- Detect drift where a reference exists.
- Score severity and dataset health.

Artifacts:

- `validation_report.json`

## Stage 6 - Scoring and Evaluation

Responsibilities:

- Compute quality scores and deltas.
- Compare detections/actions against known labels where available.
- Produce methodologically appropriate metrics.

Artifacts:

- Future evaluation summaries in `outputs/evaluation/`
- Possible confusion matrices and ROC curve images for relevant classifiers

## Stage 7 - Final Output

Expected output directory:

```text
results/
  profiling_report.json
  cleaned_data.csv
  cleaning_log.json
  validation_report.json
  logs/
  visualizations/
  evaluation/
```

This structure is proposed and may change during implementation planning.

