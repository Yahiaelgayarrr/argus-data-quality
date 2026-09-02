# Implementation Plan

Status: planned, not started.

## Critical Path

1. Approve dataset strategy and domain.
2. Finalize base technology stack.
3. Create fixtures and controlled corruption labels.
4. Implement report schemas/contracts.
5. Build Module 1 profiling.
6. Build Module 2 cleaning.
7. Build Module 3 validation.
8. Integrate modules into CLI pipeline.
9. Add Docker and data versioning.
10. Complete evaluation, documentation, and final demo.

## Prerequisites

- CadetX solo participation decision.
- Dataset/domain selection.
- Dependency approval.
- Agreement on artifact layout and report schemas.

## Module Dependencies

- Cleaning depends on profiling outputs or inferred schema.
- Validation depends on cleaned data and dataset/domain rules.
- Evaluation depends on fixtures and ground-truth labels.
- Integration depends on stable module interfaces.

## Parallelizable Work

Solo execution limits true parallelism, but planning can overlap:

- Documentation updates can happen alongside implementation.
- Evaluation fixtures can be expanded while modules mature.
- Presentation notes can be drafted after each module.

## Major Technical Risks

- Dataset does not support required demonstrations.
- Evaluation labels are weak or artificial.
- ML components add complexity without better results.
- Interface drift between modules.
- Docker environment differs from local execution.

## Decision Gates

- Gate 1: approve dataset strategy/domain before downloading or building fixtures.
- Gate 2: approve dependencies before installing.
- Gate 3: approve schemas before heavy module implementation.
- Gate 4: decide DVC vs Git LFS before storing sizable data artifacts.
- Gate 5: decide autoencoder scope after baseline anomaly detection.

