# Boolean logic, conditional branches, and table calculations in Tableau

## Boolean functions (TRUE/FALSE tests)

* **Purpose:** Create flags and filters based on data quality or membership.
* **Core tests**

  * **ISNULL(value)** → `TRUE` if value is null; else `FALSE`.
  * **ISDATE(text)** → `TRUE` if text matches a valid date pattern (locale-sensitive).
  * **expr1 IN expr2** → `TRUE` if a value exists in a set/list/combined field (e.g., `[Category] IN ("Furniture","Appliances","Decorations")`).
* **Typical uses:** Null screening, date validation, membership checks (e.g., “Top Customers” set).

## IF-family conditional logic

* **IFNULL(value, replacement):** Substitute a fallback when input is null (e.g., `IFNULL([Amount], 0)`).
* **IIF(test, out_true, out_false, [out_null]):** Ternary-style branching; optional null outcome (e.g., `IIF(SUM([Sales])>100000,"High","Low")`).
* **IF / ELSEIF / ELSE / END:** Multi-branch logic; supports compound conditions with **AND** / **OR**.
  *Example pattern:*

  ```
  IF SUM([Sales]) >= 150000 THEN "High"
  ELSEIF SUM([Sales]) >= 100000 THEN "Medium"
  ELSE "Low"
  END
  ```

## CASE statements (value matching)

* **Purpose:** Map discrete input values to labeled outputs with compact syntax.
* **Form:**

  ```
  CASE expr
    WHEN value1 THEN output1
    WHEN value2 THEN output2
    ELSE default
  END
  ```
* **Use case:** Parameter-driven labels (e.g., `[Top Customers]` → “5 Customers – Lowest value”, …).

## Table calculations (view-dependent analytics)

* **What they are:** Post-aggregate computations that operate on the marks currently in the view, ignoring filtered-out data not present in the partition.
* **Where to create:** Right-click a **measure** → **Add Table Calculation**.
* **Configuration essentials**

  * **Type:** running total, moving avg, percent of total, difference, percent difference, percentile, etc.
  * **Direction/Addressing:** across rows, down columns, or **Specific Dimensions** (explicit fields).
  * **Level of computation:** **Deepest** (default, finest grain) or selected fields; fields can be reordered to control partitioning.

### Common table-calculation patterns

* **Moving calculations (smoothing):** Sliding window over prior marks using SUM/AVG/MIN/MAX; optionally include the current mark.
* **Percent of total:** Express each mark as a share of its partition; clarify category contributions (direction matters—across vs. down).
* **Running sum (cumulative):** Accumulate marks over a dimension; can restart by a chosen dimension (e.g., per Year).
* **Difference / Percent difference:** Compare each mark with a prior mark in the partition to highlight change over time.
* **Percentile:** Rank each value from 0% to 100% within the partition.

## Custom table-calculation functions (as calculated fields)

* **INDEX():** Row position within the partition (starts at 1). Handy for top-N filters (e.g., `INDEX() <= 25`).
* **RANK() family:** Assign ranks based on an expression; nulls are ignored.

  * **RANK_DENSE:** Ties share a rank; next rank is not skipped (e.g., 1,2,2,3).
  * **RANK_MODIFIED:** Ties share a rank; sequence may skip ranks.
  * **RANK_PERCENTILE:** Returns percentile (0..1) per value.
  * **RANK_UNIQUE:** No ties; unique ranks assigned.
* **FIRST() / LAST():** Offsets from first/last row in the partition (0 at the boundary); useful for boundary-aware logic.

## Practical applications (illustrative)

* **Data quality flags:** `ISNULL([Order ID])` to filter accidental null orders.
* **Membership filters:** `[Customer Name] IN [Top Customers by Profit]` to flag priority customers.
* **Tiered labeling:** `IIF(SUM([Sales])>100000,"High Sales","Low Sales")`; extend with **IF**/**ELSEIF** for “High/Medium/Low”.
* **Trend insight:** Running sum and moving average to reveal cumulative growth and smoothed trends.
* **Comparative analysis:** Percent of total by category/year to show contribution; difference/percent difference to quantify change.

## Implementation guidelines

* **Choose the simplest construct that expresses intent:** Boolean tests for flags, `IIF` for compact binary branches, `IF` for multi-criteria logic, **CASE** for discrete value mapping.
* **Keep aggregations consistent in conditionals:** Wrap fields with the intended aggregation (e.g., `SUM`, `MIN`) when testing totals.
* **Define partitions and addressing explicitly:** Use **Specific Dimensions** and restart settings to ensure calculations align to business grain (e.g., restart per Year or Category).
* **Name for clarity and reuse:** e.g., `Flag_TopCustomer`, `Label_SalesTier`, `PctOfTotal_ByYearCategory`.
