# ADR-0005 — Polars as the core dataframe engine

- **Date:** 2026-09-27
- **Status:** Accepted

## Context

The primary dataset is 2,260,668 rows × 145 columns. Profiling touches every
column, several times: type inference, missingness, cardinality, distributions,
and a correlation matrix across all numeric pairs.

Development hardware: Intel Core Ultra 7 255HX (20 cores), 32 GB DDR5, RTX 5070
(8 GB), NVMe SSD. Multi-core capacity is the resource most worth exploiting, and
32 GB is enough to hold the dataset in a columnar format but not enough to be
careless about copies.

## Options considered

**pandas.** Universally understood; every library in the ecosystem accepts it.
But it is largely single-threaded for the operations this project performs, its
`object` dtype makes string columns memory-expensive, and its eager evaluation
means a multi-step profiling expression materialises every intermediate. At 145
columns with heavy string content, the full loan book in pandas runs to several
gigabytes of working set before any computation starts.

**Polars.** Arrow-backed columnar memory, multi-threaded by default, and — the
decisive feature — a **lazy API**: an expression chain is optimised as a whole
before execution, so projections and predicates are pushed down and unused
columns are never read. For a profiler that repeatedly asks narrow questions of a
wide table, this is the right execution model rather than merely a faster one.

**DuckDB.** An embedded analytical SQL engine that queries Parquet files
directly, without loading them. Aggregate profiling — per-column statistics,
group-bys across 11 years, correlation inputs — is naturally expressed as SQL and
executes out-of-core, so dataset size stops being bounded by RAM.

## Decision

**Polars is the core engine.** All dataset loading, transformation and the public
API surface use Polars dataframes.

**DuckDB is used for heavy aggregate profiling** where a SQL formulation is
clearer or where out-of-core execution matters. It reads the same Parquet files;
the two engines interoperate through Arrow without a copy.

**pandas is used only at library boundaries** — principally scikit-learn's
`KNNImputer` and `IterativeImputer`, which require array or pandas input. The
conversion happens at the boundary, is explicit, and is documented at each call
site. pandas is never the type carried through the engine.

**Storage format is Parquet.** The raw CSV is converted once on ingest and never
read again. Parquet gives columnar reads, per-column compression, and preserved
dtypes — which removes an entire class of spurious "defect" caused by CSV
round-tripping everything through strings.

## Consequences

**Positive**

- No sampling compromise. The full dataset is processed, so reported profiling
  figures describe the real data rather than a subsample.
- Lazy evaluation means adding a profiling metric does not linearly add a pass
  over the data.
- Parquet plus DuckDB means the pipeline is not RAM-bound, which is what makes
  the Module 4 scale test on the larger rejected-applications file feasible.

**Negative / accepted trade-offs**

- Polars is less familiar than pandas, and its API differs in real ways
  (expressions instead of chained indexing, no index, stricter typing). This is a
  learning cost, and it is part of the point.
- Three data technologies is more surface area than one. The boundaries are kept
  narrow and explicit: Polars everywhere, DuckDB for aggregates, pandas at
  scikit-learn's door and nowhere else.
- Fewer Stack Overflow answers exist for Polars edge cases.

## Note on the GPU

The RTX 5070 is not used for dataframe work — Polars is a CPU engine and 20 cores
is the relevant resource here. The GPU is used in Module 3 for the autoencoder
detector and for sentence-transformer embeddings in the NLP column classifier.
See ADR-0008 for how that affects portability.
