# Table Calculations in Tableau

## What table calculations are

* Post-query computations evaluated on the **data in the view** (after standard filters), enabling logic that spans multiple marks (rows/columns/cells).
* Distinct from:

  * **Row-level & aggregate calcs:** computed per mark at the data source.
  * **LOD expressions:** computed at specified dimensions in the data source, independent of the view.
* Processing order (simplified):

  1. Query runs at data source (includes LOD), 2) Tableau builds temporary table and applies standard filters, 3) **Table calculations run**, 4) Viz renders.

  * Earlier filters (context/data source/extract) can still affect results before table calcs execute.

## Quick Table Calculations (fast, no code)

* Available from a measure’s context menu; common options: **Running Total, Difference, % Difference, % of Total, Rank, Percentile, Moving Average, YTD Total, CAGR, YoY Growth, YTD Growth**.
* A **delta (Δ) icon** indicates a table calc is applied; edit via **Edit Table Calculation**.
* Use **Show Calculation Assistance** to visualize direction and addressing/partitioning while configuring.

## Configure “Compute Using” correctly

* Defines **addressing** (the direction marks are read) and **partitioning** (where the calc resets).
* Set with **Specific Dimensions** or layout shortcuts (e.g., **Table (Across/Down)**).
* Best practice: confirm behavior in a **crosstab** (Duplicate as Crosstab) and adjust until highlighting and results match intent.

## Custom table calculations (beyond Quick)

* Author with table-calc functions (e.g., `RANK()`, `WINDOW_AVG()`, `INDEX()`, `WINDOW_SUM()`).
* Combine with parameters, dual axes, and formatting for advanced visuals.

### Example: Bump chart (ranking over time)

* Calc: `RANK(SUM([Sales]))`.
* Place **Order Date (discrete months)** on Columns, calc on Rows, **Sub-Category** on Color.
* **Compute Using → Sub-Category** so ranks recompute per month; **Reverse** axis for rank 1 at top.
* Optional: **Dual Axis** to overlay circles/labels on lines; **Synchronize Axis** and format labels for readability.

### Example: Adjustable moving average (time-series smoothing)

* Parameter **Time Period** (integer; range 2–30; default 10).
* Calc: `WINDOW_AVG(SUM([Sales]), -[Time Period], 0)` for a trailing window.
* Overlay on original series with **Dual Axis** and **Synchronize Axis**; expose the parameter for interactive smoothing.

## Reliability and performance practices

* **Validate addressing/partitioning**: explicitly choose the dimensions to compute along; do not rely on defaults.
* **Mind filters**: standard filters run **before** table calcs; changes to filtered marks alter results. Use **context filters** when pre-filtering must affect upstream calculations.
* **Keep calcs readable**: name fields clearly (e.g., “Sales Rank (Monthly, by Sub-Category)”), and document compute-using assumptions.
* **Prefer LOD or aggregate calcs** when logic should be data-source-level or filter-independent; use table calcs when logic expressly depends on the layout (running totals, window stats, ranks in pane).
* **Test in crosstab first**, then switch to charts; confirm totals, restarts, and window bounds.
* **Use Dual Axis judiciously**: always **synchronize** and set consistent scales; adjust mark types per axis (e.g., line + circle) for clarity.
* **Expose controls** thoughtfully: parameters (e.g., window size) should have sensible ranges and defaults.

## When to use which technique

* **Running totals, moving averages, ranks, percent-of-total within pane** → Table calculations.
* **Cohort-level or fixed-dimension aggregates across the dataset** → LOD (FIXED/INCLUDE/EXCLUDE).
* **Simple per-mark math** → Row-level or aggregate calculations.

**Bottom line:** Table calculations excel at **in-view, window-aware analytics**. Correct **Compute Using** settings, deliberate filter strategy, and clear documentation are essential for accurate, maintainable results.
