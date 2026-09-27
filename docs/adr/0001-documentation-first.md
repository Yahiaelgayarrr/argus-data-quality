# ADR-0001 — Documentation-first development

- **Date:** 2026-08-23
- **Status:** Accepted

## Context

This project is built over an extended period, in intermittent sessions, partly
with AI assistance. Chat history is not durable: it is not reviewable, not
versioned, and not available to a future reader of the repository. Decisions that
live only in conversation are effectively lost, which leads to re-litigating
settled questions and to documentation that drifts away from the code.

The repository must also stand on its own as a portfolio artifact. A reader
opening it cold should be able to reconstruct what the system does and why it was
built that way without asking the author.

## Options considered

- **Chat-only planning.** Fastest to start. No durable record, no traceability.
- **One large README.** Simple, but a single file covering requirements,
  architecture, data and operations becomes unreadable and is rarely updated.
- **Focused document set with a designated entry point.** More upfront effort;
  stays navigable and reviewable as the project grows.

## Decision

Maintain a focused set of Markdown documents under `docs/`, with
[`CONTEXT.md`](../../CONTEXT.md) as the entry point that states current project
state, and this ADR directory as the durable record of decisions.

Requirements carry stable IDs (see
[`01-brief-traceability.md`](../01-brief-traceability.md)) so that code, tests and
documentation can reference the same requirement unambiguously.

## Consequences

- Planning work happens before implementation, which delays the first line of
  code and reduces the amount of code that has to be thrown away.
- Documentation must be updated in the same unit of work as the behaviour it
  describes, or it becomes actively misleading. `AGENTS.md` enforces this.
- Status language is constrained: `PLANNED` work must never be described as
  `IMPLEMENTED`. Aspirational documentation is worse than none, because a
  reviewer who finds one false claim discounts the rest.
