# ADR-0007 — Declarative rules in YAML, implementations in Python

- **Date:** 2026-09-27
- **Status:** Accepted

## Context

PIPE-003 requires YAML configuration. The open question is where the boundary
sits: does YAML configure thresholds only, or does it also declare the validation
rules themselves?

Three groups of people need to change things in this system, and they are not
equally able to change Python:

- an engineer adjusting how a rule kind works,
- a data scientist adjusting a threshold or adding a range check,
- a risk reviewer who needs to read what the rules currently are.

## Options considered

- **Rules in Python.** Maximum flexibility, fastest to write, no schema layer.
  But the set of active rules is then only discoverable by reading code, and
  changing a boundary value requires a code change and a code review.
- **Rules in YAML, logic in Python.** Rule *kinds* are implemented once in code;
  rule *instances* are declared as data.
- **Rules fully in YAML including expressions.** Ends in inventing a programming
  language inside YAML, which is a well-documented way to get the worst of both.

## Decision

**Rule kinds are implemented in Python. Rule instances are declared in YAML.**

A rule kind is a class in the engine — it knows how to evaluate a numeric range,
a cross-field ordering, a regex format, a categorical membership. A rule instance
is a YAML entry naming a kind, the columns it applies to, its parameters and its
severity.

```yaml
# configs/rules/lending.yaml  (illustrative, not final)
rules:
  - id: LOAN-AMNT-RANGE
    kind: numeric_range
    column: loan_amnt
    min: 500
    max: 40000
    severity: critical

  - id: CREDIT-HISTORY-ORDER
    kind: date_order
    before: earliest_cr_line
    after: issue_d
    severity: critical

  - id: GRADE-SUBGRADE-CONSISTENCY
    kind: derived_consistency
    parent: grade
    child: sub_grade
    relation: prefix
    severity: warning
```

Configuration is **validated on load** against a schema, with errors that name
the offending file, rule ID and field. A config error must fail immediately and
legibly, not surface later as a mysteriously absent check.

Anything a rule cannot express declaratively becomes a **new rule kind in
Python**, never an escape hatch that evaluates arbitrary expressions from YAML.
Executing config as code would make the config a security boundary, and it is not
one.

## What lives in config versus code

| In YAML | In Python |
| --- | --- |
| Which rules are active | How each rule kind evaluates |
| Column names and parameters | The engine, profiler, cleaner |
| Thresholds and severities | Scoring formulae |
| Imputation strategy per column | Imputer implementations |
| Health-score weights | Report schemas |
| Paths and run options | Everything else |

## Consequences

**Positive**

- The active rule set is readable as a document, which is what the compliance
  persona actually needs.
- Adding a range check to a new column is a config commit, reviewable on its own.
- Rule configs are per-dataset artifacts, which is precisely what makes the
  engine/pack split in ADR-0006 work in practice.

**Negative / accepted trade-offs**

- A config schema and loader must be built and tested before the first rule runs.
- Two files to read to understand one rule.
- A rule that does not fit an existing kind is more work than a quick function
  would be. This is the intended pressure: it keeps rule kinds few and general.
