# Tableau calculations—types, uses, and aggregation

## Purpose and scope

* Extend data models by creating **calculated fields** that behave like native fields across Tableau platforms (Desktop, Server, Prep).
* Enable dynamic analytics through **table calculations**, which respond to view structure and user interactions.
* Support both **row-level logic** and **aggregation-aware logic** so results are correct at the intended level of detail.

## Core calculation types

* **Calculated fields (row/aggregation aware):** Custom expressions that can be used in shelves, filters, other calcs, reference lines, etc. Example: `Total Sales = [Quantity] * [Cost Price]`.
* **Table calculations:** Post-aggregate computations that depend on table layout and direction (across/down). Can be stored as fields and configured for address/partition settings.
* **Level of Detail (LOD) calculations:** Fix computation at explicit dimensionality (e.g., **FIXED** customer for first order date) to decouple results from the current view.

## Data-type and operator behavior

* Expressions obey **data types**; mixing incompatible types is invalid.

  * Example: numeric `2 + 2 → 4`; string `"2" + "2" → "22"`.
* Conversion functions are available; **date** and **spatial** fields can be created from other types.

## Aggregation principles (critical)

* Decide whether logic is **row-level** or **aggregate**:

  * Row-level example: `[Field] + 2` adjusts each record.
  * Aggregate example: `SUM([Field]) + 2` adds to the total only.
* If **any** referenced field is aggregated in a calc, **all** referenced fields must be aggregated (or wrapped to match).
* Pre-aggregating within a calculated field fixes the aggregation used in the view (no alternate aggregation selection later).

## Built-in aggregation functions (with examples)

* **SUM(num)**: total, ignores nulls — e.g., `SUM([Sales])`.
* **AVG(num)**: mean, ignores nulls — e.g., `AVG([Sales])`.
* **MIN(value1, [value2]) / MAX(value1, [value2])**: works on numbers, dates, strings; with two inputs returns pairwise min/max; with one input returns dataset min/max.
* **COUNT(value)**: count of non-null rows; **COUNTD(value)**: distinct count.
* **ATTR(value)**: returns the sole value across the partition, else `*` (useful for “should be single-valued” checks).
* **COLLECT(spatial)**: aggregates geospatial marks.

**Illustrative use cases**

* **Sales per unit**: `SUM([Sales]) / SUM([Quantity])`.
* **First order date**: `MIN([Order Date])`, or fixed per entity with LOD.

## Statistical & mathematical functions

* **MEDIAN(num)**, **PERCENTILE(field, p)**.
* **STDEV(field) / STDEVP(field)**: sample vs. population standard deviation.
* **VAR(field) / VARP(field)**: sample vs. population variance.
* **CORR(x, y)**: Pearson correlation (−1..1).
* **COVAR(x, y) / COVARP(x, y)**: sample/population covariance.
* General math: square roots, rounding, handling nulls, **π**, **sin/cos/tan**, etc.

## String, date, boolean, and regex capabilities

* **String/text**: positional parsing, case changes, substring search, **regex** for complex patterns.
* **Date**: extract parts (year/month/day), construct dates from strings/numbers/now, round/truncate to period, add/subtract time units.
* **Boolean/conditional**: tests for null, membership (`IN`), and branching via **IF/THEN/ELSE** or **CASE**.

## Authoring workflow (reliable pattern)

1. **Create** via Data pane ▸ **Create Calculated Field**; consult the built-in function reference and syntax help.
2. **Declare intent**: choose row-level vs. aggregate vs. LOD/table calc based on the question.
3. **Align types**: ensure compatible data types or convert explicitly.
4. **Enforce aggregation consistency**: wrap fields to a common level (e.g., `SUM`, `MIN`) to avoid mismatches.
5. **Validate in the view**: drop results onto shelves, confirm totals vs. per-row behaviors, and adjust addressing/partitioning for table calcs.

## Practical guidance

* Prefer **aggregate formulations** for ratios and rates to avoid low-level bias (e.g., `SUM(Profit)/SUM(Sales)` instead of row-wise averages).
* Use **FIXED LOD** to anchor metrics to business grain (e.g., first order date per customer) and safely compare against row dates.
* Choose **table calculations** for view-dependent analytics (running totals, percent of total, moving stats) and document their compute direction.
* Treat **type conversions and null handling** explicitly to prevent silent errors in concatenation or math.
* Keep calculations modular and named for intent (e.g., `Sales_per_Unit`, `First_Order_Date_Fixed`) to encourage reuse and reduce ambiguity.
