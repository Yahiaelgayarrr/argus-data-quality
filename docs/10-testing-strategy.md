# Testing Strategy

Status: planned, not implemented.

## Principles

- Tests begin with implementation, not at the end.
- Every module should have deterministic fixtures.
- ML evaluation should use ground truth where available and documented assumptions where not.
- Bugs should be reproduced with tests where practical.

## Test Levels

| Level | Purpose | Examples |
| --- | --- | --- |
| Unit tests | Verify focused functions/classes. | Type inference, missingness stats, email validator, quality score calculation. |
| Module integration tests | Verify a complete module output. | Profiling writes valid JSON; cleaning writes CSV/log; validation writes report. |
| End-to-end tests | Verify full pipeline orchestration. | CLI reads sample CSV and writes all expected artifacts. |
| Evaluation tests | Verify metrics and ground-truth comparisons. | Duplicate precision/recall, imputation error, anomaly confusion matrix. |
| Regression tests | Prevent known bugs from returning. | Add after discovered defects. |

## Fixtures

Planned fixture types:

- Small clean dataset.
- Dirty deterministic dataset.
- Controlled corruption dataset with labels.
- Edge-case dataset with empty columns, mixed types, all-null columns, duplicate rows, invalid dates, and invalid categories.

## Module Completion Gates

### Module 1

- Unit tests for profiling components.
- JSON schema validation for `profiling_report.json`.
- Fixture confirms missingness, type, cardinality, and PII flags.

### Module 2

- Unit tests for imputers, normalizers, duplicate detection, and scoring.
- Integration test writes `cleaned_data.csv` and `cleaning_log.json`.
- Evaluation records imputation/duplicate metrics.

### Module 3

- Unit tests for validation rules.
- Integration test writes `validation_report.json`.
- Evaluation records anomaly and semantic-classification metrics where ground truth exists.

### Module 4

- CLI end-to-end test.
- Docker smoke test.
- Config loading test.
- Logging behavior test.

## CI Verification

Potential future CI:

```bash
pytest
python pipeline.py --input tests/fixtures/dirty_sample.csv --output outputs/test-run/
```

These commands are planned, not currently runnable.

