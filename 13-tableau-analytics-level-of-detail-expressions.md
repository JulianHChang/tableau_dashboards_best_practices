# Leveraging Level of Detail (LOD) calculations in Tableau

## What LOD calcs solve

* Perform aggregations at a **specified granularity** that can differ from the view’s granularity, enabling comparisons across levels (row, view, higher/lower grain) without rebuilding the view.
* Typical use cases: cohort/period comparisons, true overall benchmarks, first/last event attributes, segment vs portfolio deltas, per-entity flags based on history.

## Levels of detail to distinguish

* **Data LOD (grain):** one record’s meaning (e.g., one customer per row). Row-level calcs operate here.
* **View LOD:** distinct combo of dimensions placed in the view; aggregate calcs operate here.
* **Calculated LOD:** granularity declared in an LOD expression—independent of the view.

## Syntax

```
{ FIXED | INCLUDE | EXCLUDE [Dim1], [Dim2], ... : AGG([Field]) }
```

* `FIXED` ignores view dimensions and computes at the listed ones (or whole table if none).
* `INCLUDE` adds listed dimensions to the view’s granularity for the calc.
* `EXCLUDE` removes listed dimensions from the view’s granularity for the calc.
* Result returns at row level and can be reused in other calcs, on shelves, or as filters.

## Core patterns with concise examples

### 1) True overall benchmarks (FIXED)

* Problem: Reference lines on an aggregated view often reflect the **average of aggregates**, not the **overall average**.
* Solution:

  ```tableau
  {FIXED : AVG([Orders])}
  ```

  Use as a reference line/benchmark to compare per-state sums against the dataset-wide average.

### 2) Historical flags across records (FIXED)

* Goal: Flag any member who **ever** dropped below a credit-score threshold, and persist that flag on all rows for that member.

  ```tableau
  {FIXED [Member ID] : MIN([Credit Score])} < 550
  ```
* Notes:

  * Fixed LODs are **context-sensitive**: they respect **context filters** but ignore non-context filters. Add filters to context when the fixed scope must narrow.

### 3) First/last event attributes (FIXED)

* Goal: Keep only the **latest balance per member/loan** (or first).

  ```tableau
  // latest
  {FIXED [Member ID], [Loan Number] : MAX([Date])} = [Date]
  // first (swap MAX for MIN)
  ```
* Use as a boolean filter or to build before/after comparisons.

### 4) Detail below the view (INCLUDE)

* Goal: **Average loans per member by state** (view at State, calc at Member).

  ```tableau
  {INCLUDE [Member ID] : COUNTD([Loan Number])}  // then AVG in the view
  ```
* Alternatives:

  * Aggregate approach:

    ```
    COUNTD(STR([Member ID]) + "_" + STR([Loan Number])) / COUNTD([Member ID])
    ```
  * Equivalent FIXED (when each member belongs to one state):

    ```
    {FIXED [State], [Member ID] : COUNTD([Loan Number])}
    ```
  * Consider `MAX([Loan Number])` if loan numbers increment sequentially per member—often faster.

### 5) Rollups above the view (EXCLUDE)

* Goal: Compare **loan-type** average credit score to the **portfolio** average.

  ```tableau
  // portfolio-level average
  [Average Excl Loan Type] = {EXCLUDE [Loan Type] : AVG([Credit Score])}
  // difference from portfolio
  AVG([Credit Score]) - AVG([Average Excl Loan Type])
  ```
* Produces per-type deltas centered on the portfolio benchmark.

## Design & performance guidance

* **Choose the simplest LOD** that answers the question:

  * `INCLUDE` for calculations needing finer grain than the view.
  * `EXCLUDE` for higher-level rollups while keeping detailed marks.
  * `FIXED` for stable, repeatable scopes (entity, portfolio, or whole table).
* **Mind filter order (Tableau order of operations):**

  * Context filters run **before** FIXED; non-context filters run **after**. Promote critical filters to context when FIXED scope must follow them.
* **Minimize dimensions in FIXED** to reduce computation and improve extract performance.
* **Validate with small crosstabs** to confirm grain, distinct counts, and averages before styling charts.
* **Name fields descriptively** (e.g., `Avg Loans per Member (INCLUDE)`, `Portfolio Avg (EXCLUDE)`) to aid reuse and auditing.
* **Test edge cases** (members with multiple states, duplicate records, missing dates) to ensure the chosen grain is correct.

Applied well, LOD expressions make multi-granular analysis clear, performant, and maintainable—unlocking precise benchmarks, robust flags, and trustworthy comparisons within a single, coherent view.
