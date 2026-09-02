# Project Scope

## In Scope

- Tabular dataset ingestion from CSV during the initial implementation.
- Profiling metadata, types, missingness, cardinality, distributions, correlations, suspicious columns, inconsistent formats, and potential PII.
- Cleaning missing values, duplicates, strings, dates, categorical values, types, and selected transformations.
- ML-based imputation where justified by the dataset and evaluation design.
- Rule-based validation for emails, phones, postcodes, numeric ranges, and categories.
- Anomaly detection with Isolation Forest and Local Outlier Factor.
- Semantic column classification and mislabeled-column checks.
- Data drift detection if a baseline/reference dataset or split can be defined.
- Quality scoring before cleaning, after cleaning, and after validation.
- JSON/CSV artifacts required by the official brief.
- Reproducible tests, fixtures, and evaluation methodology.
- CLI, configuration, logging, Docker, and GitHub-oriented documentation.

## Out of Scope Unless Requirements Change

- Real-time streaming ingestion.
- Full web application or dashboard.
- Enterprise data catalog integration.
- Database write-back connectors.
- Paid cloud deployment.
- Handling every possible file format at production scale.
- Training large language models.
- Autoencoder implementation unless time and evaluation evidence justify it.
- Committing real personal data or sensitive datasets.

## Optional Stretch Goals

- Support for Excel or Parquet input after CSV is stable.
- Lightweight HTML report generation.
- GitHub Actions CI.
- DVC-based dataset/version tracking.
- Domain-specific validation packs.
- Autoencoder anomaly detection.
- A richer CLI subcommand structure.

## Scope Control Rule

Any new feature must be mapped to an official requirement, approved stretch goal, or documented decision. Planned features must not be described as implemented.

