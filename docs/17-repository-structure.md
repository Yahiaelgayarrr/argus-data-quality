# Initial Repository Structure

Status: proposed initial structure.

The repository should stay small enough to understand while still supporting the full three-month project.

```text
.
|-- README.md
|-- AGENTS.md
|-- context.md
|-- CHANGELOG.md
|-- docs/
|   |-- 00-official-requirements.md
|   |-- 01-project-overview.md
|   |-- 02-project-scope.md
|   |-- 03-functional-requirements.md
|   |-- 04-non-functional-requirements.md
|   |-- 05-system-architecture.md
|   |-- 06-data-flow.md
|   |-- 07-dataset-strategy.md
|   |-- 08-technology-stack.md
|   |-- 09-project-decisions.md
|   |-- 10-testing-strategy.md
|   |-- 11-evaluation-strategy.md
|   |-- 12-12-week-roadmap.md
|   |-- 13-implementation-plan.md
|   |-- 14-current-status.md
|   |-- 15-risk-register.md
|   |-- 16-agent-skills-audit.md
|   |-- 17-repository-structure.md
|   |-- modules/
|   |-- architecture/
|   |-- experiments/
|   |-- reports/
|   `-- presentation/
|-- src/
|-- tests/
|-- data/
|-- configs/
|-- scripts/
|-- notebooks/
|-- outputs/
`-- .github/
```

## Directory Responsibilities

| Path | Purpose |
| --- | --- |
| `README.md` | Public entry point and current high-level project summary. |
| `context.md` | Master project index and current-state handoff for humans and AI agents. |
| `AGENTS.md` | Operating rules for AI coding agents. |
| `CHANGELOG.md` | Meaningful project-level history. |
| `.gitignore` | Prevents accidental commits of caches, local secrets, datasets, and generated outputs. |
| `docs/` | Authoritative planning, architecture, requirements, status, and methodology documentation. |
| `docs/modules/` | Module specifications and future module documentation. |
| `docs/architecture/` | Detailed architecture notes and interface contracts when needed. |
| `docs/experiments/` | Reproducible experiment records and templates. |
| `docs/reports/` | Curated final reports, not routine generated outputs. |
| `docs/presentation/` | Module and final presentation notes. |
| `src/` | Future Python package source. Empty during Phase 0. |
| `tests/` | Future pytest tests and fixtures. Empty during Phase 0. |
| `data/` | Future dataset workspace. Must avoid committing sensitive or large files accidentally. |
| `configs/` | Future YAML configs and dataset/domain rule packs. |
| `scripts/` | Future utility scripts, such as corruption generation or evaluation helpers. |
| `notebooks/` | Optional exploratory notebooks, if approved. |
| `outputs/` | Generated pipeline outputs. Usually ignored or curated carefully. |
| `.github/` | Future GitHub Actions and repository workflow templates. |

## Structure Rule

Do not add directories just because they are common. Add them when a real artifact or workflow needs a clear home.
