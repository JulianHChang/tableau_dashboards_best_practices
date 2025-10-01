# Level-of-Detail (LOD) calculations in Tableau

## What LODs solve

* Compute metrics at a **specified granularity** that can differ from the viz level, unlike table calcs (which operate over dimensions in the view).
* Ideal for stable benchmarks, per-entity flags, rollups above/below the view, and multi-granular comparisons in a single chart.

## Types & mental model

* **INCLUDE** — *add* finer detail to the view’s granularity for the calc (more granular, less aggregated).
* **EXCLUDE** — *remove* detail from the view’s granularity for the calc (less granular, more aggregated).
* **FIXED** — compute at the exact granularity listed, **independent** of the view.

> Granularity = dimensions that define the computation.

## Syntax (always aggregated)

```
{ INCLUDE | EXCLUDE | FIXED [Dim1], [Dim2], ... : AGG([Field or expression]) }
```

* Curly braces `{}` wrap the expression.
* Optional comma-separated dimension list precedes the colon `:`.
* Right side must be an **aggregation** (e.g., SUM, AVG, MIN, MAX, COUNTD, COUNT) or an already-aggregated expression.
* Returns a value that can then be further aggregated in the view.

## Order of operations

* **INCLUDE/EXCLUDE** are evaluated **after** standard dimension filters.
* **FIXED** is evaluated **before** most filters and respects **context filters**. Promote filters to **Context** when a FIXED LOD must honor them.

## Canonical patterns (with concise examples)

### 1) INCLUDE — average sales per product within sub-category (without showing product)

* Goal: average of per-product sums at the sub-category level.

  ```tableau
  // LOD returns SUM(Sales) per Product within the current Sub-Category
  { INCLUDE [Product Name] : SUM([Sales]) }
  // In the view: wrap with AVG to get “avg sales per product” by Sub-Category
  ```
* Behavior: responds to ordinary filters (e.g., Order Date) because INCLUDE is post-filter.

### 2) EXCLUDE — percent contribution by state within region

* Goal: compute regional total while keeping State in the view; then derive contribution.

  ```tableau
  // Regional total repeated for each State in Region
  [Regional Sales] = { EXCLUDE [State] : SUM([Sales]) }
  // Contribution %
  [State % of Region] = SUM([Sales]) / [Regional Sales]
  ```
* Advantage over table calcs: no “compute using” tuning; dimensions explicitly controlled.

### 3) FIXED — stable totals/benchmarks unaffected by dimension filters

* Goal: same percent-of-region scenario, but **fixed** against filtering (e.g., show only Texas).

  ```tableau
  [Regional Sales (Fixed)] = { FIXED [Region] : SUM([Sales]) }
  [State % of Region (Fixed)] = SUM([Sales]) / [Regional Sales (Fixed)]
  ```
* Notes: values remain stable under regular filters; add filters to **Context** to affect FIXED results.

### 4) Table-scoped FIXED

* Omit dimensions to compute against the whole table:

  ```tableau
  { FIXED : AVG([Sales]) }   // same as { AVG([Sales]) }
  ```

## Nested LODs

* Chain LODs to roll up from one level to another.

  * Example: *max state sales by region*:

    ```tableau
    // Inner: per-State sales
    [Sales (State)] = { FIXED [State] : SUM([Sales]) }

    // Outer: max of those state totals per Region
    [Max State Sales by Region] = { FIXED [Region] : MAX([Sales (State)]) }
    ```
* Use cases: “best/worst within group”, “value vs group max/avg”, first/last event by group, etc.
* Keep nesting shallow (usually ≤2 levels) for clarity and maintainability.

## Quick LODs (productivity)

* **Ctrl/⌘ + drag** a **measure** onto a **dimension** in the Data pane → creates `{ FIXED [Dim] : <default agg>([Measure]) }`.
* Repeat to nest (e.g., drag the first quick LOD onto another dimension).
* Adjust default aggregation on the field to influence Quick LOD output (e.g., set to MAX before nesting).

## Formatting & reuse tips

* Set **default number formats** (e.g., percentage) for derived fields to ensure consistent presentation.
* Name fields descriptively (e.g., `Avg Sales per Product (INCLUDE)`, `Region Total (EXCLUDE)`).
* Validate with a simple crosstab before styling the viz.

## Performance & correctness guidelines

* Minimize dimensions in FIXED to limit computation scope.
* Prefer COUNTD only when necessary; consider surrogate keys or faster equivalents (e.g., `MAX` if IDs are sequential per entity).
* Verify data grain/duplicates; FIXED can help neutralize duplication from joins/relationships.
* Align filters with intent: context filters for FIXED, regular filters for INCLUDE/EXCLUDE.

**Bottom line:** LOD expressions provide precise, declarative control over calculation grain—enabling robust benchmarks, rollups, and per-entity logic that stay correct as views change, filters apply, and analyses evolve.
