# Project Context

This is the first file future humans and AI agents should read.

## Project Identity

- Project: CadetX - Automated Data Cleaning & Validation System
- Repository: `cadetx-data-quality-pipeline`
- Phase: Phase 0 - requirements, planning, architecture
- Work mode: Solo execution, pending CadetX approval where the official brief expects a team of three
- Current date initialized: 2026-08-23

## Official Source

The authoritative project source is the attached CadetX programme brief for "Automated Data Cleaning & Validation System." Requirement extraction is preserved in [docs/00-official-requirements.md](docs/00-official-requirements.md).

## Objective

Build a backend data-quality gatekeeper that can profile, clean, validate, and run an end-to-end automated pipeline for a tabular dataset before internal use.

## Current Scope

The planned system includes four modules:

1. Profiling and metadata intelligence
2. Cleaning and transformation
3. AI validation, anomaly detection, and rule validation
4. Integration, orchestration, CLI, Docker, and documentation

See [docs/02-project-scope.md](docs/02-project-scope.md).

## Current Architecture Summary

Proposed architecture:

```text
Input dataset
-> Dataset loader
-> Profiling engine
-> Cleaning engine
-> Validation engine
-> Quality scoring
-> Reports and final artifacts
```

Detailed architecture is in [docs/05-system-architecture.md](docs/05-system-architecture.md) and [docs/06-data-flow.md](docs/06-data-flow.md).

## Module Status

| Area | Status |
| --- | --- |
| Foundation documentation | IN PROGRESS |
| Dataset selection | OPEN |
| Module 1 - Profiling | NOT STARTED |
| Module 2 - Cleaning | NOT STARTED |
| Module 3 - Validation | NOT STARTED |
| Module 4 - Integration | NOT STARTED |
| Tests | NOT STARTED |
| Docker | NOT STARTED |
| Evaluation | PLANNED |
| Final presentation | NOT STARTED |

Authoritative status: [docs/14-current-status.md](docs/14-current-status.md).

## Important Decisions

No final technical decisions have been made beyond using a documentation-first process and adapting team process requirements for solo work. Open decisions are tracked in [docs/09-project-decisions.md](docs/09-project-decisions.md).

## Dataset

No dataset has been selected. Dataset strategy options are analyzed in [docs/07-dataset-strategy.md](docs/07-dataset-strategy.md).

## Technology Stack

Python is expected, but dependencies remain proposed or open until approved. See [docs/08-technology-stack.md](docs/08-technology-stack.md).

## Verification Commands

No code verification commands exist yet. Planned future commands:

```bash
pytest
python pipeline.py --input data.csv --output results/
```

These are not currently runnable.

## Documentation Map

- Requirements: [docs/00-official-requirements.md](docs/00-official-requirements.md)
- Overview: [docs/01-project-overview.md](docs/01-project-overview.md)
- Scope: [docs/02-project-scope.md](docs/02-project-scope.md)
- Functional requirements: [docs/03-functional-requirements.md](docs/03-functional-requirements.md)
- Non-functional requirements: [docs/04-non-functional-requirements.md](docs/04-non-functional-requirements.md)
- Architecture: [docs/05-system-architecture.md](docs/05-system-architecture.md)
- Data flow: [docs/06-data-flow.md](docs/06-data-flow.md)
- Dataset strategy: [docs/07-dataset-strategy.md](docs/07-dataset-strategy.md)
- Technology stack: [docs/08-technology-stack.md](docs/08-technology-stack.md)
- Decisions: [docs/09-project-decisions.md](docs/09-project-decisions.md)
- Testing: [docs/10-testing-strategy.md](docs/10-testing-strategy.md)
- Evaluation: [docs/11-evaluation-strategy.md](docs/11-evaluation-strategy.md)
- Roadmap: [docs/12-12-week-roadmap.md](docs/12-12-week-roadmap.md)
- Implementation plan: [docs/13-implementation-plan.md](docs/13-implementation-plan.md)
- Current status: [docs/14-current-status.md](docs/14-current-status.md)
- Risks: [docs/15-risk-register.md](docs/15-risk-register.md)
- Agent skills audit: [docs/16-agent-skills-audit.md](docs/16-agent-skills-audit.md)
- Repository structure: [docs/17-repository-structure.md](docs/17-repository-structure.md)
- Module specs: [docs/modules/](docs/modules/)

## Known Limitations

- Solo participation approval is not yet confirmed.
- Dataset/domain has not been selected.
- Dependency stack has not been finalized.
- No pipeline code, tests, reports, Dockerfile, or CI exists yet.

## Current Blockers

- CadetX solo participation confirmation.
- Dataset/domain decision.
- Technology stack approval.

## Next Recommended Action

Review the Phase 0 documentation, then decide:

1. Whether to proceed with the recommended hybrid dataset strategy.
2. Which domain/dataset candidates to investigate first.
3. Whether to approve the proposed base Python stack.
