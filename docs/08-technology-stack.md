# Technology Stack

Status: proposed. No dependencies have been installed.

| Technology | Purpose | Why | Alternatives | Required? | Decision |
| --- | --- | --- | --- | --- | --- |
| Python | Main implementation language | Official brief names Python; strong data ecosystem | R | Yes | PROPOSED |
| pandas | Tabular data manipulation | Standard for CSV profiling/cleaning | Polars, DuckDB | Likely | OPEN |
| NumPy | Numeric operations | Underpins pandas/scikit-learn | Python stdlib, scipy | Likely | OPEN |
| scikit-learn | KNN/iterative imputation, Isolation Forest, LOF, metrics | Official ML requirements align well | PyOD, custom implementations | Likely | OPEN |
| scipy | Statistical helpers | Useful for distributions/correlations | NumPy only | Optional | OPEN |
| pydantic or dataclasses | Report/config schemas | Keeps JSON contracts explicit | Marshmallow, plain dicts | Optional | OPEN |
| PyYAML or ruamel.yaml | YAML configuration | Official brief requires YAML config | TOML, JSON | Likely | OPEN |
| matplotlib/seaborn | Backend visualizations | Missing/outlier/correlation heatmaps | Plotly, Altair | Likely | OPEN |
| pytest | Automated tests | Official brief requires pytest | unittest | Yes | PROPOSED |
| rapidfuzz | Fuzzy duplicate detection | Fast fuzzy matching | difflib, fuzzywuzzy | Optional | OPEN |
| phonenumbers | Phone validation | Robust international phone parsing | Regex only | Optional | OPEN |
| Docker | Reproducible containerized execution | Official Module 4 requirement | Podman | Yes | PROPOSED |
| GitHub Actions | CI verification | Free automation for tests | Local-only scripts | Optional but useful | OPEN |
| DVC or Git LFS | Data versioning | Official brief asks for one | Manual checksums | Yes, choice open | OPEN |
| Jupyter notebooks | Exploration and experiment explanation | Useful for learning and evaluation | Scripts only | Optional | OPEN |

## Dependency Rule

No dependency should be added until it solves a concrete project requirement and is recorded here with a final decision.

