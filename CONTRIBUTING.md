# Contributing

Standards for this repository. Written for a solo developer, but written as if for
a team — because that is the point ([ADR-0002](docs/adr/0002-solo-execution.md)).

---

## Git workflow

**One branch per work unit.** A work unit is a coherent change that leaves the
repository in a working state. Not a calendar period.

```
master          always works; every commit is releasable
  └─ docs/*     documentation and planning
  └─ feat/*     new capability
  └─ fix/*      corrections
  └─ test/*     test-only changes
  └─ chore/*    tooling, dependencies, CI
  └─ refactor/* behaviour-preserving restructuring
```

Never commit directly to `master`.

### Commit messages

```
<type>(<scope>): <what changed, imperative>

Why this change was needed. The diff already shows what changed;
the message exists to explain the reasoning that the diff cannot.

Requirements: PROF-004, PROF-013
```

Types: `feat`, `fix`, `docs`, `test`, `refactor`, `chore`, `perf`.

Reference requirement IDs whenever the commit advances one. That is what makes the
traceability matrix checkable rather than aspirational.

### Pull requests

Every change reaches `master` through a PR, even self-reviewed. The PR description
is the durable record and must cover:

1. **What** changed
2. **Why** — the problem being solved
3. **Requirements** advanced, by ID
4. **How it was verified** — tests, recorded results, manual checks
5. **Trade-offs** accepted, and what was deliberately left out
6. **Follow-ups**, if any

A PR description reading "added profiling" is a wasted artifact. The PR history is
read as evidence.

### Self-review checklist

Work through this before merging. It is not a formality — it is the only review
this code gets.

**Correctness**
- [ ] Tests pass locally (`pytest`)
- [ ] New behaviour has a test that fails without the change
- [ ] Edge cases considered: empty dataset, single row, all-null column,
      single-value column, all-unique column, mixed types, non-UTF-8

**Scope**
- [ ] The diff contains only what the PR describes
- [ ] No unrelated refactoring, no drive-by reformatting
- [ ] No debugging output or commented-out code left behind

**Architecture**
- [ ] Business logic is in the engine, not in a CLI or API adapter
- [ ] No dataset-specific column name appears anywhere in `src/argus/`
- [ ] Layer boundaries respected ([ADR-0006](docs/adr/0006-package-first-architecture.md))

**Data safety**
- [ ] No data file is committed — no CSV, Parquet, model weights
- [ ] No real personal data in fixtures, tests, or example outputs
- [ ] Log messages and report contents do not leak sensitive column values

**Documentation**
- [ ] `CONTEXT.md` status reflects reality
- [ ] Requirement statuses updated, and not inflated
- [ ] A new significant decision has an ADR
- [ ] `CHANGELOG.md` updated
- [ ] Docstrings explain *why*, not *what*

**Honesty**
- [ ] Nothing is described as `IMPLEMENTED` that is only `PLANNED`
- [ ] Nothing is described as `VERIFIED` without evidence
- [ ] Known limitations are stated, not omitted

## Code standards

### Tooling

| Tool | Role |
| --- | --- |
| `ruff` | Linting and import ordering |
| `black` | Formatting — no style debates |
| `mypy` | Type checking on `src/argus/` |
| `pytest` | Tests |
| `pre-commit` | Runs the above before each commit |

Configuration lives in `pyproject.toml`. Line length 88.

### Python conventions

- **Type hints on every public function.** They are the cheapest documentation
  available.
- **Docstrings on public functions**, explaining purpose, arguments, returns, and
  crucially *why this approach*. Internal helpers need only a line if their name is
  not self-evident.
- **No bare `except`.** Catch a specific exception or let it propagate.
- **No silent failures.** A function that cannot do its job raises or returns an
  explicit result — never a quiet default that looks like success.
- **Pure functions where practical.** Take data, return data. Side effects
  (file writes, logging) live at the edges.
- **Small, single-purpose modules.** A module needing the word "and" to describe it
  is two modules.

### Naming

Names should say what a thing means in the problem domain, not how it is
implemented. `detect_mixed_type_columns` over `check_cols`. `co_missingness_matrix`
over `nan_corr`.

### Comments

Comment the surprising, not the obvious.

```python
# Bad — restates the code
# loop over the columns
for column in columns:

# Good — records domain knowledge that is not visible in the code
# A null in mths_since_last_delinq means the borrower has never been
# delinquent, not that the value is unknown. Imputing it would fabricate
# a delinquency history, so it is flagged as structurally missing instead.
```

## Testing

See `docs/07-testing.md` (planned) for the full strategy. The rules that apply from
the first commit:

- **Every test uses committed fixtures**, never the real dataset. The suite must
  run on a fresh clone with no download.
- **Fixtures are deliberately nasty:** mixed types, unicode, empty strings versus
  nulls, a column that is entirely null, a column that is entirely unique.
- **A bug fix starts with a failing test** that reproduces it.
- **Randomness is seeded.** An unseeded test that passes most of the time is worse
  than no test.
- **Never weaken an assertion to make a test pass.** Either the code is wrong or the
  assertion was wrong; decide which, and say so.

## Documentation

Documentation changes in the **same commit** as the behaviour it describes. Not the
next commit.

- `CONTEXT.md` — updated whenever status changes
- `CHANGELOG.md` — updated per work unit
- `docs/adr/` — a new record for each significant decision, never an edit to an
  accepted one. Decisions that change get a new ADR and the old one is marked
  superseded.
- `docs/modules/` — written as the module is built, not retrospectively

## Dependencies

Before adding one:

1. Does it solve a concrete requirement?
2. Is it maintained?
3. Is it free?
4. What is the alternative, including writing it yourself?

Record it in `docs/04-tech-stack.md` with the answer to 4. Pin the version. A
dependency added without a recorded reason will be removed by a future reader who
cannot tell why it is there.
