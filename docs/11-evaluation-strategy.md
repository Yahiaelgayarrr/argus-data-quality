# Evaluation Strategy

Status: planned, not implemented.

## Goal

Avoid judging success by whether the pipeline produces files. Each intelligent or corrective component should be evaluated with suitable evidence.

## Ground Truth Sources

- Controlled corruption labels for injected missing values, duplicates, invalid values, semantic errors, and anomalies.
- Original clean values for masked-value imputation evaluation.
- Hand-labeled column semantic types for semantic classification.
- Deterministic validation fixtures for rule-based validators.
- Before/after quality metrics for cleaning impact.

## Component Evaluation

| Component | Evaluation Method | Metrics |
| --- | --- | --- |
| Type inference | Compare inferred types to labeled fixture schema. | Accuracy, error analysis. |
| Semantic classification | Compare predicted semantic labels to hand labels. | Accuracy, precision/recall where multi-label. |
| Missing-value detection | Deterministic checks. | Count correctness. |
| Imputation | Mask known values and compare reconstruction. | MAE/RMSE for numeric, accuracy for categorical. |
| Duplicate detection | Inject known exact/fuzzy duplicates. | Precision, recall, F1. |
| Normalization | Dirty-to-clean expected fixtures. | Pass/fail transformation checks. |
| Rule validation | Known invalid values. | Precision, recall, F1 where labels exist. |
| Anomaly detection | Inject or label anomalies. | Precision/recall, confusion matrix, ROC/AUC where scores and labels support it. |
| Drift detection | Compare baseline vs shifted dataset. | Detected drift features, statistical test outcomes where appropriate. |
| Quality scoring | Compare before/after and explain components. | Quality delta and score decomposition. |
| Full pipeline | End-to-end run with expected artifacts. | Artifact completeness, schema validity, test pass rate. |

## Methodological Risks

- Synthetic corruption may be too easy or unrealistic.
- Ground truth may leak into model/config design.
- Anomaly detection can be subjective without labels.
- ROC curves only make sense when labels and continuous scores exist.
- ML methods may not beat simpler baselines.

## Evaluation Rule

Use metrics only where they are meaningful. For deterministic cleaning and validation, exact fixture tests may be stronger than ML-style metrics.

