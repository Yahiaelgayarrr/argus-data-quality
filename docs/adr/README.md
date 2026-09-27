# Architecture Decision Records

Each ADR records one decision: the context, the options considered, what was
chosen, and the trade-off accepted. ADRs are append-only. When a decision
changes, the old record stays and its status becomes `Superseded by ADR-XXXX`.

Reading the ADRs in order should explain *why* this system looks the way it
does, not just what it does.

## Status vocabulary

| Status | Meaning |
| --- | --- |
| `Proposed` | Under discussion, not yet acted on |
| `Accepted` | In force; the codebase reflects it |
| `Superseded` | Replaced by a later ADR, kept for the record |
| `Deferred` | Deliberately postponed, with a stated trigger for revisiting |

## Index

| ADR | Title | Status |
| --- | --- | --- |
| [0001](0001-documentation-first.md) | Documentation-first development | Accepted |
| [0002](0002-solo-execution.md) | Solo execution adaptation | Accepted |
| [0003](0003-domain-and-dataset.md) | Domain and dataset: consumer lending | Accepted (supersedes the healthcare choice) |
| [0004](0004-hybrid-ground-truth.md) | Hybrid real data plus injected ground truth | Accepted |
| [0005](0005-polars-core-engine.md) | Polars as the core dataframe engine | Accepted |
| [0006](0006-package-first-architecture.md) | Package-first architecture with thin interfaces | Accepted |
| [0007](0007-declarative-rule-config.md) | Declarative rules in YAML, implementations in Python | Accepted |
| [0008](0008-interface-sequencing.md) | Interface build order: CLI, then HTML, then API, then dashboard | Accepted |
| [0009](0009-data-versioning-tool.md) | Data versioning with DVC | Accepted |

## Writing a new ADR

Copy the structure of an existing file. Number sequentially. Keep it to one
decision per record — if a file needs the word "also", it is two ADRs.
