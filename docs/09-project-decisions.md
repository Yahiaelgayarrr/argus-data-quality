# Project Decisions

Important decisions are recorded here. Do not remove historical decisions because plans changed; update status instead.

## ADR-001 - Documentation-First Development

- Date: 2026-08-23
- Status: ACCEPTED
- Decision: The repository will preserve requirements, architecture, plans, status, and agent instructions before implementation.
- Context: Future AI agents may have no chat history and must be able to recover project state from the repository.
- Options considered: chat-only planning; single large README; focused documentation system.
- Chosen approach: focused Markdown documentation with `context.md` as the entry point.
- Reasoning: Reduces context loss, documentation drift, and false implementation claims.
- Consequences: More upfront work; easier review and safer future implementation.

## ADR-002 - Solo Execution Adaptation

- Date: 2026-08-23
- Status: ACCEPTED, pending CadetX approval for solo participation
- Decision: Preserve technical requirements while adapting team-only process requirements to solo execution.
- Context: Official CadetX brief assumes a team of three and rotating Scrum Master. User intends to work alone.
- Options considered: ignore team process; simulate team roles; adapt to solo evidence.
- Chosen approach: solo weekly planning, documented self-review, disciplined Git history, and per-module documentation.
- Reasoning: Maintains evidence of consistency and engineering quality without inventing teammates.
- Consequences: Collaboration criterion remains a risk unless CadetX approves solo work.

## DECISION-001 - Dataset Domain and Dataset

- Date: 2026-08-23
- Status: OPEN
- Problem: The system needs a dataset that demonstrates many quality issues and supports objective evaluation.
- Options: naturally messy dataset; clean dataset plus controlled corruption; hybrid.
- Current recommendation: hybrid.
- Needed from user: approve dataset strategy and choose candidate domain(s).

## DECISION-002 - Data Versioning Tool

- Date: 2026-08-23
- Status: OPEN
- Problem: Module 4 requires data versioning with DVC or Git LFS.
- Options: DVC; Git LFS.
- Needed from user: choose after dataset size and workflow are known.

## DECISION-003 - Dependency Stack

- Date: 2026-08-23
- Status: OPEN
- Problem: Core dependencies must support profiling, cleaning, ML, validation, testing, and Docker.
- Current recommendation: Python, pandas, NumPy, scikit-learn, pytest, PyYAML, matplotlib/seaborn, with optional rapidfuzz and phonenumbers.
- Needed from user: approve base stack before installation.

## DECISION-004 - Autoencoder Scope

- Date: 2026-08-23
- Status: OPEN
- Problem: Autoencoder anomaly detection is listed as advanced in the brief, but may add complexity.
- Options: implement; defer; evaluate only if time permits.
- Current recommendation: defer until Isolation Forest and LOF are evaluated.

