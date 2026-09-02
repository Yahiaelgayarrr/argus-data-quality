# Dataset Strategy

Decision status: RESOLVED (2026-09-02). Domain: healthcare. Dataset: CMS "Medicare Physician & Other Practitioners - by Provider and Service" (public dataset), held locally by the user pending transfer into `data/raw/`.

## Strategy A - Naturally Messy Real-World Dataset

Pros:

- Realistic issues and portfolio credibility.
- Natural distributions and domain constraints.
- Useful for demonstrating exploratory profiling.

Cons:

- Ground truth is often missing.
- Harder to evaluate imputation, duplicate detection, anomaly detection, and cleaning accuracy objectively.
- Licensing and privacy may be more complex.

Best fit: final demo realism, profiling, qualitative validation.

## Strategy B - Clean Dataset + Controlled Corruption

Pros:

- Known injected errors create ground truth.
- Strong evaluation for missingness, duplicates, invalid values, semantic errors, and anomalies.
- Reproducible experiments.

Cons:

- May feel artificial if used alone.
- Synthetic errors may not match real messy data.

Best fit: evaluation-first development and test fixtures.

## Strategy C - Hybrid

Pros:

- Combines real-world shape with known injected corruption.
- Supports objective metrics while preserving realism.
- Best alignment with portfolio and evaluation goals.

Cons:

- Requires a corruption generator and careful documentation.
- Need to avoid leakage from knowing injected labels during model design.

Recommendation: hybrid. Approved.

## Candidate Domains

| Domain/Dataset Direction | Fit | Feature Types | Quality Problems | Evaluation Potential | Complexity | Licensing | Portfolio Value |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Retail/customer orders | Strong | Numeric, dates, categories, names/emails/addresses | Missing values, duplicates, invalid ranges, inconsistent categories, PII-like fields | High with synthetic corruption | Medium | Must verify | Strong business relevance |
| HR employee records | Strong | Dates, IDs, categories, salaries, departments | PII risk, category drift, impossible values | High with synthetic data or public sample | Medium | Must avoid real PII | Strong compliance angle |
| Healthcare appointments | Strong but sensitive | Dates, categories, numeric measures, patient-like fields | Missingness, invalid ranges, PII risk | High with synthetic/public data | High | Must verify carefully | Strong but privacy-sensitive |
| Finance transactions | Strong | Numeric, dates, categories, IDs | Outliers, duplicates, suspicious patterns | High anomaly potential | Medium-high | Must verify | Strong anomaly demo |
| Sports player statistics | Moderate | Numeric/categorical/time | Outliers, missing values, inconsistent names | Moderate; less PII | Low-medium | Often permissive | Good explainability, weaker validation breadth |

## Current Recommendation

Use a hybrid strategy:

1. Select a real public dataset with varied columns and clear licensing.
2. Preserve an immutable raw version.
3. Create a controlled corruption generator that injects labeled errors.
4. Use injected labels as evaluation ground truth.
5. Keep a small deterministic fixture dataset for tests.

## Chosen Dataset

- Domain: Healthcare
- Dataset: CMS "Medicare Physician & Other Practitioners - by Provider and Service"
- Status: chosen by the user (2026-09-02), currently local-only (not yet added to this repository)
- Open items: verify CMS licensing/usage terms, confirm file size, decide DVC vs Git LFS (DECISION-002) before committing it to `data/raw/`

