# Filtering in Tableau — types, setup, scope, and order of operations

## Filter types and when to use them

* **Dimension filters** (discrete values): Include/exclude members regardless of view aggregation; can be built from the view via *Keep Only* / *Exclude*. If multiple dimensions are selected in the view, Tableau creates a **set** to filter In/Out membership.
* **Measure filters** (numeric ranges): Choose **row-level** (*All Values*) or **aggregate** filtering (SUM, AVG, MEDIAN, COUNT, COUNTD, MIN, MAX, STDEV/VAR variants). Aggregate filters adapt to the view’s level of detail.
* **Date filters**:

  * **Relative Date** (e.g., last 3 years, current month, anchor month).
  * **Range of Dates** or **Discrete parts** (exact dates, year/quarter/month/week/day, combinations).
  * **Aggregated date** filtering mirrors measure filters (COUNT, COUNTD, MIN, MAX).
* **Table calculation filters**: Execute **after** the view (post-aggregate), filtering marks based on table-calc results (e.g., `INDEX()` top-N). Do not change underlying data; can be applied to totals via **Apply to Totals**.

## Configure filters effectively

* **From the view**: Lasso bars/marks → *Keep Only* / *Exclude*; Tableau creates the corresponding filter.
* **Show Filter** for interactivity; choose control style: single/multi-select list or dropdown, single value slider, custom list, wildcard search.
* **Measure filter UI**: set **range**, **at least**, **at most**; optionally **Include Null Values**; pick **Only Relevant Values** vs **All Values in Database**.
* **Dimension filter UI**:

  * **General** tab: check/uncheck members; toggle **Exclude**.
  * **Wildcard** tab: *Contains*, *Starts with*, *Ends with*, *Exact* (supports dynamic matching like area codes or suffixes).
  * **Condition** tab: *By field* (aggregated comparison) or *By formula* (Boolean expression).
  * **Top** tab: Top/Bottom **N** by chosen measure or formula.

### Practical patterns

* **Wildcard**: ends-with “Class” to keep all shipping methods that match now and in future refreshes.
* **Condition**: `SUM([Sales]) < 100000` to keep low-sales sub-categories.
* **Top-N**: Top 20 states by `SUM([Sales])`; dynamically updates as data changes.
* **Table-calc filter**: `INDEX() ≤ 5` to keep the top five *positionally*; results change with sort because table calcs compute last.

## Apply filters across worksheets (scope)

* Default scope is **single sheet**. Broaden by right-clicking the filter → **Apply to Worksheets**:

  * **All Using Related Data Sources** (same or related via blending; shows linked database icon).
  * **All Using This Data Source** (white database icon).
  * **Selected Worksheets** (stacked-sheets icon).
    The applied filter overwrites existing filters on the same field in target sheets.

## Order of operations (why results differ)

1. **Extract filters** — reduce rows written into extracts.
2. **Data source filters** — pre-filter incoming data before any sheet logic.
3. **Context filters** — promoted filters that run early; affect sets, FIXED LODs, conditional and Top-N logic.
4. **Dimension filters** — include/exclude members (after the above).
5. **Measure filters** — after include/exclude LODs and blending.
6. **Table calculation filters** — last; applied to already-computed marks (forecasts, table calcs, clusters, totals computed before them; trend/reference lines after).

### Context filters in practice

* Use **Add to Context** (filter turns gray) to ensure a normal filter runs **before** Top-N or FIXED LODs.

  * Example: to see Top-5 **within Office Supplies**, make **Category** a **context** filter so Top-N is computed inside that category.

## Governance and usability

* Prefer **dynamic** criteria (Relative Date, Top-N, Wildcards, Conditions) to avoid manual maintenance.
* Name filters descriptively and document intent (e.g., captions or field descriptions).
* Align **filter grain** with analytical grain: use aggregate measure/date filters for view-level comparisons; use row-level filtering only when appropriate.
* For performance, push reduction **upstream**: extract filters and data source filters minimize data volume early.
* When using table-calc filters for positional logic, communicate that results respond to **sort** and **partition** settings.

## Representative workflows

* **Exclude outliers quickly**: select marks → *Exclude*; Tableau adds a dimension filter.
* **Wildcard category maintenance**: Wildcard “Ends with ‘Class’” avoids manual updates as new ship modes arrive.
* **Cohort focus across a workbook**: apply a **Category** filter to **Selected Worksheets** to synchronize dashboards.
* **Pre-filter large datasets**: apply a **Data Source Filter** (e.g., Category = Furniture) so downstream filters and lists reflect the reduced domain.
