# ADR-0006 — Package-first architecture with thin interfaces

- **Date:** 2026-09-27
- **Status:** Accepted

## Context

This system will eventually be reachable four ways: a CLI (required by PIPE-006),
a FastAPI service, a React dashboard, and interactively from notebooks during
development.

The failure mode to avoid is obvious once named: each interface grows its own copy
of the orchestration logic. The CLI does profiling one way, the API does it
slightly differently, and after a few weeks the two disagree about what "clean"
means. This happens by default when interfaces are written as scripts.

## Options considered

- **Script-per-task.** A folder of runnable `.py` files. Fastest to start; no
  reusable boundary, awkward imports, logic duplicated per entry point.
- **Package with a public API, interfaces as thin adapters.** More setup;
  one implementation, many front doors.

## Decision

Build an installable package at `src/argus/`, installed in editable mode
(`pip install -e .`), exposing a small stable public API. Every interface is an
adapter over that API.

```
Layer 3   CLI    FastAPI    React (via API)    notebooks
             \      |         /                 /
              \     |        /                 /
Layer 2        argus.profile() / clean() / validate() / run_pipeline()
                            |
Layer 1        internal engine modules
```

**Layer 2 is the product.** Each public function takes data plus a config object
and returns a typed result. It does no argument parsing, no printing, no HTTP, and
no file-path guessing.

**Layer 3 adapters are deliberately trivial.** A CLI command reads flags,
constructs a config, calls Layer 2, writes output. An API endpoint does the same
with a request body. If an adapter grows branching business logic, that logic
belongs in Layer 1 and the adapter is wrong.

**`src/` layout, not a top-level package directory.** This forces the test suite
to import the installed package rather than accidentally importing from the
working directory, which is the difference between testing what ships and testing
what happens to be lying around.

## Engine and domain pack separation

Cutting across the layers: the engine is dataset-agnostic, and lending-specific
knowledge lives in a separable domain pack.

- **Engine** provides rule *kinds* — a numeric range rule, a cross-field
  ordering rule, a categorical membership rule.
- **Domain pack** provides rule *instances* — `loan_amnt` must fall within the
  platform's permitted range; `earliest_cr_line` must precede `issue_d`.

Nothing in `src/argus/` may hard-code a Lending Club column name. This is
verifiable: grep the engine for lending identifiers, and the result should be
empty. Module 4's generalisation run on a different dataset is the real test.

## Consequences

**Positive**

- Adding the FastAPI layer in Module 4 is adapter work, not a rewrite.
- The public API is testable without going through a CLI or an HTTP client.
- The engine/pack split makes "works on any dataset" a demonstrable claim rather
  than a README assertion.

**Negative / accepted trade-offs**

- `pyproject.toml`, packaging metadata and an editable install are setup cost
  before any feature exists.
- Indirection: reading a rule end-to-end means reading the engine's rule kind and
  the pack's instance, in two places.
- Keeping the engine genuinely domain-free requires discipline at exactly the
  moments when hard-coding a column name would be quicker.
