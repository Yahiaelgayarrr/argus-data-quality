# Non-Functional Requirements

## CadetX-Required Constraints

| ID | Requirement | Source | Status |
| --- | --- | --- | --- |
| NFR-001 | Repository work must be updated in GitHub. | PROC-004 | PLANNED |
| NFR-002 | Engineering quality must be visible through structured modules and clean code. | Evaluation criteria | PLANNED |
| NFR-003 | Documentation must include module documentation, architecture, API usage, and pipeline flow. | PROC-006, PIPE-011 | PLANNED |
| NFR-004 | The final system must be runnable end-to-end and demonstrable. | PIPE-012 | PLANNED |
| NFR-005 | Free tools should be used. | PROJ-005 | PLANNED |

## Engineering Quality Targets

| ID | Requirement | Rationale |
| --- | --- | --- |
| NFR-006 | Reproducibility: commands, config, seeds, and outputs should be repeatable. | Supports evaluation and portfolio defense. |
| NFR-007 | Maintainability: modules should have clear responsibilities and small interfaces. | Reduces integration failure. |
| NFR-008 | Testability: behavior should be covered with fixtures and automated tests. | Prevents silent regressions. |
| NFR-009 | Privacy: real PII must not be committed; generated reports should avoid leaking sensitive values. | Data-quality systems often inspect sensitive columns. |
| NFR-010 | Observability: logs and reports should explain what happened and why. | Cleaning must be auditable. |
| NFR-011 | Portability: local and Docker execution should behave consistently. | Required for demo reliability. |
| NFR-012 | Performance: initial target is practical performance on demo-scale datasets, not big-data scale. | Keeps scope realistic. |
| NFR-013 | Documentation freshness: docs must change when requirements, behavior, interfaces, datasets, or status change. | Prevents stale source-of-truth docs. |

