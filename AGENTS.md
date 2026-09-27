# Operating Manual

Instructions for anyone — human or AI — working in this repository.

**The repository is the source of truth, not chat history.** Conversations are not
versioned, reviewable, or available to the next reader. If a decision matters, it
is written down here or it does not exist.

---

## 1. Read before doing anything

1. [`CONTEXT.md`](CONTEXT.md) — current state: what exists, what is in progress,
   what is next
2. [`docs/00-charter.md`](docs/00-charter.md) — what the project is for and what
   is out of scope
3. [`docs/01-brief-traceability.md`](docs/01-brief-traceability.md) — the
   requirement IDs everything references
4. [`docs/adr/`](docs/adr/) — decisions already made, and why
5. The relevant module document in [`docs/modules/`](docs/modules/)
6. The code and tests for the area being changed

If a task appears to contradict an accepted ADR, stop and raise it. Do not quietly
implement the contradiction.

## 2. Status vocabulary

Use these exact words. They mean specific things.

**Requirement type**

| Term | Meaning |
| --- | --- |
| `REQUIRED` | Stated or clearly implied by the CadetX brief |
| `EXTENDED` | This project's addition, beyond the brief |
| `OPTIONAL` | Useful, not required |
| `AMBIGUOUS` | The brief is unclear; interpretation is recorded |

**Implementation status**

| Term | Meaning |
| --- | --- |
| `PLANNED` | Intended. No code. |
| `IN PROGRESS` | Partially implemented, not yet complete |
| `IMPLEMENTED` | Code exists and runs |
| `VERIFIED` | Code exists **and** a passing test or recorded result proves it |
| `DEFERRED` | Deliberately postponed, with a stated reason and trigger |
| `BLOCKED` | Cannot proceed without a decision or external change |

**The rule that matters most:** never describe `PLANNED` work as `IMPLEMENTED`,
and never describe `IMPLEMENTED` work as `VERIFIED` without evidence. A reviewer
who finds one inflated claim discounts every other claim in the repository.
Optimistic documentation is worse than none.

## 3. Before writing code

State, briefly and explicitly:

1. Which requirement IDs this work addresses
2. What is in scope for this change and what is not
3. The assumptions being made
4. The acceptance criteria — how it will be known to work
5. Which files are expected to change
6. Which tests are required

If any of the six cannot be answered, the task is not ready to start.

## 4. While writing code

- **Make surgical changes.** No opportunistic refactoring of untouched code.
- **Respect the layer boundaries** of [ADR-0006](docs/adr/0006-package-first-architecture.md).
  Business logic lives in the engine. CLI and API adapters parse input, call the
  public API, and write output — nothing more.
- **Never hard-code a dataset-specific column name in `src/argus/`.** Lending
  knowledge belongs in the domain pack under `configs/`. This is checkable by
  grepping the engine for lending identifiers; the result must be empty.
- **Do not silently change** requirements, architecture, interfaces,
  configuration, dependencies, or dataset assumptions. Propose, then change.
- **Do not add a dependency** without recording it in
  [`docs/04-tech-stack.md`](docs/04-tech-stack.md) with a reason and the
  alternatives considered.
- **Do not download or move datasets** without saying so first.
- **Never commit data.** No CSV, no Parquet, no model weights. See
  [ADR-0009](docs/adr/0009-data-versioning-tool.md). The only exception is the
  small committed test fixture.
- **Do not hide failures to make tests pass.** A skipped test with a reason is
  honest; a weakened assertion is not.
- **Write the "why" in the code**, not the "what". A comment explaining that a
  missing `mths_since_last_delinq` means "never delinquent" rather than "unknown"
  is worth more than a comment saying a function imputes values.

## 5. After writing code

Run the tests. Read the diff. Then check whether the change affects any of:

- requirements or their status
- architecture or interfaces
- configuration or its schema
- dependencies
- dataset assumptions
- evaluation methodology
- `CONTEXT.md`
- `README.md`
- `CHANGELOG.md`

Anything affected is updated **in the same commit**. Documentation that lags the
code by even one commit is how a repository starts lying.

## 6. Cadence and Git

This project is paced by **work unit, not calendar**
([ADR-0002](docs/adr/0002-solo-execution.md)). A work unit is a coherent change
that leaves the repository in a working state. It may take an hour or a month.

**Never write a week-numbered schedule into this repository.**

Git discipline, per [`CONTRIBUTING.md`](CONTRIBUTING.md):

- One branch per work unit
- Commit messages explain *why*, not just what
- Pull request with a written description, then the self-review checklist, then
  merge
- `master` always works

## 7. Learning is an objective, not a side effect

The purpose of this project is to understand every technique it uses. That changes
how work is done here:

- When a technique is introduced, the module document explains **what it does, why
  it was chosen over the alternatives, and what its failure modes are** — before
  the implementation.
- "It works" is not a sufficient justification for a choice.
- If a simpler approach would have sufficed, say so and explain what the more
  complex one bought.

Code that works but is not understood is a liability in this repository, not an
asset.
