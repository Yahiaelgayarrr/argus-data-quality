# Agent Skills Audit

Status: audit only. No skills or plugins have been installed.

The current Codex environment already exposes several relevant skills. This audit recommends the smallest useful set to rely on or consider later. It does not approve installation of any new dependency, plugin, or skill.

| Skill | Purpose | Source | Why useful here | Overlap | Risk/Maintenance | Recommendation |
| --- | --- | --- | --- | --- | --- | --- |
| data-analytics:analyze-data-quality | Assess data quality and evidence reliability. | Installed curated plugin skill | Directly aligned with profiling, validation, and evidence quality. | Overlaps with validate-data. | Plugin behavior may change; keep repo docs authoritative. | CONSIDER |
| data-analytics:validate-data | Review analysis methodology and conclusions. | Installed curated plugin skill | Useful before presenting evaluation results. | Overlaps with analyze-data-quality. | Should not replace tests or documented evaluation. | CONSIDER |
| data-analytics:jupyter-notebooks | Create reproducible SQL/Python notebooks. | Installed curated plugin skill | Useful for experiment notebooks if notebooks become part of workflow. | Overlaps with normal Python scripts. | Notebooks can drift from production code. | CONSIDER |
| data-analytics:visualize-data | Build and QA quantitative charts. | Installed curated plugin skill | Useful for evaluating profiling/validation charts. | Overlaps with matplotlib/seaborn work. | Avoid polished visuals that do not reflect real results. | CONSIDER |
| data-analytics:build-report | Build analytical reports. | Installed curated plugin skill | Useful near final portfolio/demo stage. | Overlaps with README and presentation docs. | Could overproduce report artifacts early. | SKIP for now |
| pdf:pdf | Create/read/verify PDFs. | Installed primary-runtime skill | Useful only if final report or exported presentation is PDF. | Overlaps with Markdown docs. | Adds unnecessary workflow now. | SKIP for now |
| presentations:Presentations | Create/edit slide decks. | Installed primary-runtime skill | Useful for final CadetX presentation. | Overlaps with Markdown presentation notes. | Too early before results exist. | CONSIDER later |
| spreadsheets:Spreadsheets | Create/edit/analyze spreadsheet files. | Installed primary-runtime skill | Useful if dataset exploration uses Excel/CSV artifacts. | Overlaps with pandas. | Not needed for core backend implementation. | SKIP for now |
| openai-docs | OpenAI/Codex product docs and settings. | Installed system skill | Useful only for Codex/tooling questions. | None for project code. | Not a data-quality skill. | SKIP for project work |
| skill-installer | Install curated or GitHub skills. | Installed system skill | Useful only if a missing high-value engineering skill is identified. | Overlaps with manual guidance. | Installing unnecessary skills increases process complexity. | SKIP unless requested |
| Codex Security plugin | Security review capability if installed. | Recommended plugin list | Could help with secrets/PII/security review later. | Overlaps with manual security review. | Requires plugin installation and permissions. | CONSIDER later |
| GitHub plugin | Repository/PR/issue integration if available. | Available connector family in environment | Useful if remote GitHub workflow needs direct management. | Overlaps with git CLI. | Account permissions; not needed until remote work starts. | CONSIDER later |

## Recommended Minimal Set

No new skill installation is recommended for Phase 0.

For later phases, rely on existing built-in/relevant skills only when they materially help:

- `data-analytics:analyze-data-quality` for data-quality assessment.
- `data-analytics:validate-data` before final evaluation claims.
- `presentations:Presentations` near final demo preparation.

## Installation Recommendation

INSTALL: none now.

CONSIDER:

- Data Analytics quality/evaluation skills already available.
- Presentations skill near Week 12.
- Codex Security plugin later if the repository begins handling sensitive-looking data or secrets.

SKIP:

- Broad plugin installation.
- Notebook/report/presentation skills before results exist.
- Any tool that creates maintenance burden without solving a current requirement.

