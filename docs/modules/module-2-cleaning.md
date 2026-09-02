# Module 2 - Cleaning

Status: specification only. Not implemented.

## Objective

Clean and transform the dataset after profiling while preserving an auditable record of every meaningful change.

## Requirement IDs

CLEAN-001 through CLEAN-014.

## Inputs

- Raw dataset.
- Profiling output or inferred schema.
- Cleaning configuration.

## Outputs

- `cleaned_data.csv`
- `cleaning_log.json`

## Components

- Schema inference
- Missing-value handler
- Statistical imputer
- KNN imputer
- Regression/iterative imputer
- Duplicate detector
- Type normalizer
- String/date/categorical normalizers
- Transformation engine
- Quality scorer
- Cleaning log writer

## Algorithms and Methods

- Mean/median/mode imputation where appropriate
- KNN imputation for numeric feature spaces
- Regression or iterative imputation after leakage review
- Fuzzy matching for near-duplicate records
- Configurable normalization rules

## Interfaces

Cleaning should accept input data plus config and return cleaned data plus a structured cleaning log.

## Dependencies

Likely: pandas, NumPy, scikit-learn, rapidfuzz optional.

## Evaluation

- Mask known values and measure imputation reconstruction.
- Use known duplicate pairs for duplicate metrics.
- Compare before/after quality score.

## Tests

- Unit tests for imputers, normalizers, duplicate detection, and scoring.
- Integration test for `cleaned_data.csv` and `cleaning_log.json`.

## Risks

- Cleaning may damage valid unusual values.
- ML imputation may leak target information.
- Fuzzy matching thresholds may over-merge records.

## Definition of Done

- Required cleaning actions implemented and logged.
- Cleaned data and log artifacts generated.
- Quality delta produced.
- Tests and evaluation pass.
- Documentation updated.

