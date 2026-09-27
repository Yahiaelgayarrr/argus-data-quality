# ADR-0002 — Solo execution adaptation

- **Date:** 2026-08-23 (solo participation confirmed 2026-09-02)
- **Status:** Accepted

## Context

The CadetX brief assumes a team of three with a weekly rotating Scrum Master,
weekly team meetings, and a split between an AI Engineer discipline and a Data
Scientist discipline. This project is executed by one person covering both
disciplines across all four modules.

The brief's assessment criteria include *Collaboration*, defined as real GitHub
teamwork — commits, pull requests, reviews, merges — and Scrum participation.
That criterion cannot be satisfied literally without a team.

## Options considered

- **Ignore the process requirements.** Loses the Collaboration criterion
  entirely and produces a messy single-branch history.
- **Simulate a team.** Invent teammates or sock-puppet reviewers. Dishonest, and
  transparently so to anyone reading the commit history.
- **Adapt team mechanics to solo equivalents.** Keep every practice whose value
  survives without a second person; drop the ones that are purely coordination.

## Decision

Preserve all technical requirements unchanged. Replace team-coordination
mechanics with solo equivalents that produce the same evidence:

| Brief expectation | Solo equivalent |
| --- | --- |
| Rotating Scrum Master | Dropped. Pure coordination overhead with one person. |
| Weekly team meeting | Dropped as a meeting. Replaced by a written progress note when a work unit closes. |
| Branches, PRs, reviews, merges | Kept in full. Feature branches, PRs with written descriptions, and a documented self-review checklist before merge. |
| Per-module presentation | Kept in full. One document per module under `docs/modules/`. |
| Weekly progress cadence | Replaced by per-work-unit progress. See below. |
| Final demo to mentor | Kept in full. |

## Cadence

The brief frames progress as weekly over three months. This project is paced by
**work unit, not by calendar**: a unit is a coherent, reviewable change that
leaves the repository in a working state. Units may take a day or several weeks.

Consistency is therefore evidenced by the commit and PR record and by
`CHANGELOG.md`, not by a fixed weekly rhythm. No document in this repository
should contain a week-numbered schedule.

## Consequences

- Pull requests are self-reviewed. The self-review checklist in
  `CONTRIBUTING.md` makes that a real step rather than a rubber stamp, but it is
  weaker than independent review and is acknowledged as such.
- One person covering both disciplines means the AI/ML layer and the
  statistics/profiling layer are built sequentially rather than in parallel.
- The `Collaboration` criterion is addressed through Git hygiene and written
  review artifacts. This is stated openly in the README rather than papered over.
