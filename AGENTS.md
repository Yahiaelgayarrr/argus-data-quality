# AI Agent Operating Manual

This repository is documentation-first. Future AI agents must treat repository files, not chat history, as the project source of truth.

## Required Reading Order

Before coding, read:

1. [context.md](context.md)
2. [docs/00-official-requirements.md](docs/00-official-requirements.md)
3. [docs/14-current-status.md](docs/14-current-status.md)
4. The relevant module specification in [docs/modules/](docs/modules/)
5. [docs/05-system-architecture.md](docs/05-system-architecture.md)
6. [docs/06-data-flow.md](docs/06-data-flow.md)
7. [docs/09-project-decisions.md](docs/09-project-decisions.md)
8. Relevant tests and implementation files, once they exist

## Status Language

Use these statuses consistently:

- REQUIRED: stated or strongly implied by the CadetX brief
- OPTIONAL: useful but not required
- AMBIGUOUS: unclear in the official brief
- PLANNED: intended but not implemented
- IMPLEMENTED: code exists
- VERIFIED: implementation has passing evidence
- DEFERRED: intentionally postponed
- BLOCKED: cannot proceed without a decision or external change

Never describe PLANNED functionality as IMPLEMENTED.

## Before Coding

For every implementation task:

1. Inspect the repository.
2. Identify relevant requirement IDs.
3. Identify relevant module specification.
4. State task scope and assumptions.
5. Define acceptance criteria.
6. Identify expected files to change.
7. Identify tests required.

## During Coding

- Make surgical changes.
- Follow existing architecture and module boundaries.
- Prefer simple, testable designs.
- Avoid unrelated refactoring.
- Do not silently change requirements, architecture, configuration, datasets, or dependencies.
- Do not install dependencies without approval.
- Do not download datasets without approval.
- Do not commit or push unless explicitly asked.
- Do not hide errors just to make tests pass.

## After Coding

Run relevant verification, inspect diffs, and update documentation if behavior changed.

Ask whether the change affects:

- requirements
- architecture
- interfaces
- configuration
- dependencies
- dataset assumptions
- evaluation
- module status
- current status
- README
- agent instructions

If yes, update the appropriate document in the same work unit.

## Solo-Work Adaptation

The official brief assumes a team of three and rotating Scrum Master. This repo is being executed individually. Preserve the technical requirements, GitHub evidence, documentation, weekly progress discipline, and final demo requirements. Replace team-only mechanics with solo equivalents such as weekly planning notes, self-review checklists, and PR-style branches if useful.

