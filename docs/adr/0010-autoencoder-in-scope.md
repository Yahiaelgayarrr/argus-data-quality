# ADR-0010 — Autoencoder anomaly detection moved into scope

- **Date:** 2026-09-27
- **Status:** Accepted
- **Supersedes:** DECISION-004 (autoencoder deferred), recorded 2026-08-23

## Context

The brief lists Isolation Forest and Local Outlier Factor as required anomaly
detectors, and an autoencoder as an *advanced* technique to attempt if justified.
An earlier decision deferred it, on the reasoning that a deep model was
disproportionate effort for a component whose required alternatives already
existed.

Two things have changed since that decision, and both cut the other way.

**The hardware exists.** Development runs on an RTX 5070. Training a small
autoencoder over a tabular dataset of this size is minutes of GPU time, not the
multi-day exercise that made deferral attractive.

**The evaluation harness exists.** ADR-0004 commits to a corruption injector that
labels every defect it plants. That turns "is the autoencoder worth it?" from an
opinion into a measurement: it can be scored against Isolation Forest and LOF on
the same labels, on the same data. Without labels, adding a third detector would
only add a third set of unfalsifiable claims.

There is also a reason specific to the detector itself. Isolation Forest and LOF
score a row by how isolated it sits in the feature space. An autoencoder scores it
by how badly the row is reconstructed after being compressed through a bottleneck,
which makes it sensitive to a **broken relationship between columns** rather than
to an extreme value in one of them. A loan whose `installment` disagrees with its
own amortisation terms may be unremarkable on every individual column and still
fail to reconstruct. That is a different question about the data, not a fancier
way of asking the same one — which is the only argument that justifies a third
detector.

## Options considered

- **Keep it deferred.** Honest, satisfies the brief, costs nothing. But it leaves
  the one genuinely advanced technique in the brief unattempted, and the reason
  for deferring it (cost) no longer holds.
- **Implement it, GPU only.** Fastest path. Breaks PIPE-007: the Docker image
  must run on a machine with no GPU, and a demo that only runs on the author's
  laptop is not a delivered system.
- **Implement it with a mandatory CPU fallback.** Chosen.

## Decision

**VAL-006 is `REQUIRED` in this project**, not optional, and is implemented with a
mandatory CPU fallback path.

- Device selection is by **runtime capability detection**, not configuration. A
  config flag would let the container be misconfigured into requesting a GPU that
  is not there; capability detection cannot be got wrong by the operator.
- The architecture stays small and is recorded, not tuned into significance: a
  shallow undercomplete autoencoder over scaled numeric and encoded categorical
  columns.
- **It must beat its baselines to be reported as useful.** It is evaluated on the
  same injected labels, with the same metrics, as Isolation Forest and LOF. If it
  does not improve detection, the result is reported as a negative finding and the
  detector stays in the repository as evidence of the comparison. A technique that
  did not pay off is a legitimate result; quietly dropping it, or reporting it
  without the comparison, is not.
- Training seeds are fixed, so a reported score is reproducible.

## Consequences

**Positive**

- The brief's advanced option is attempted rather than skipped, with its value
  measured rather than asserted.
- Adds a detector sensitive to cross-column structure, which the two required
  detectors are not.
- The CPU fallback keeps the Docker image portable, which PIPE-007 requires
  regardless of this ADR.

**Negative / accepted trade-offs**

- PyTorch becomes a dependency of the engine, and it is a heavy one. It inflates
  the Docker image substantially.
- Two code paths for one detector means the CPU path needs its own test, or it
  will rot unnoticed on a GPU development machine.
- An autoencoder's reconstruction error is harder to explain to the compliance
  persona than a rule violation is. Its output must therefore carry the columns
  that contributed most to the error, not only a score.
- This is scope the brief did not require. It must never be worked on while a
  `REQUIRED` row elsewhere is unfinished.
