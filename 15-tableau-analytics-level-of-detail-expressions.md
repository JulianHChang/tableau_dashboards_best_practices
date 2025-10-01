# Level of Detail (LOD) expressions in Tableau

## Purpose and advantages

* LOD expressions explicitly set the **aggregation grain** of a calculation, independent of the dimensions present in the view.
* Unlike table calculations (computed over dimensions in the viz), LODs are **pushed to the data source**, often improving performance and ensuring only aggregated results return to Tableau.
* Enable side-by-side comparisons across different grains within the same visualization (e.g., per-customer metrics vs. per-region metrics).

## Syntax and behavior

```
{ <LOD keyword> <Dimension(s)> : <Aggregate Calculation> }
```

* **Keywords**

  * **FIXED** — compute at the listed dimensions regardless of the view; ignores regular filters and respects **context/data-source/extract** filters.
  * **INCLUDE** — add listed dimensions to the view’s grain for the calc (finer granularity).
  * **EXCLUDE** — remove listed dimensions from the view’s grain for the calc (coarser granularity).
* The right side must be an **aggregate** (e.g., SUM, AVG, MIN, MAX, COUNTD, COUNT) or an expression that returns an aggregate.

## Core patterns with examples

### 1) Cohort analysis (customer first-purchase year)

* **Goal:** Group customers by first purchase and analyze contribution over time without adding customer to the view.
* **Calc:**
  `{ FIXED [Customer ID] : MIN([Order Date]) }  // First Purchase`
* **Use:** Color bars of sales by order year using *First Purchase* to reveal cohort revenue contribution (e.g., strong impact from the 2015 cohort).

### 2) Regional averages by customer (two-stage mean)

* **Goal:** Compare **average sales per customer** across regions (not the average transaction value).
* **Calc:**
  `{ INCLUDE [Customer Name] : SUM([Sales]) }  // Sales per Customer`
  Then place on the view as **AVG([Sales per Customer])** by Region.
* **Insight:** Average transaction value may look similar across regions, while **per-customer** averages can diverge meaningfully (e.g., East/West outperform Central/South).

### 3) National context within regional bars (coarser grain for reference)

* **Goal:** Show **regional sales by sub-category** while also encoding **national sub-category sales** for context.
* **Calc:**
  `{ EXCLUDE [Region] : SUM([Sales]) }  // Sales across Regions`
* **Use:** Color regional bars by national sub-category totals to spot under/over-performance (e.g., chairs weak in South despite strong national sales).

## Practical guidance

* **Choose the right keyword**

  * Use **FIXED** for stable benchmarks and cohort keys that must not change with ordinary filters.
  * Use **INCLUDE** to compute lower-level rollups (e.g., per-customer) and then re-aggregate (e.g., average) at the view level.
  * Use **EXCLUDE** to add higher-level context (e.g., portfolio/region totals) to granular views.
* **Filter semantics**

  * Expect **FIXED** to ignore standard dimension filters; convert relevant filters to **Context** to make FIXED respond.
  * **INCLUDE/EXCLUDE** honor regular filters in the view.
* **Performance & clarity**

  * Keep **dimension lists minimal** in FIXED to reduce compute scope.
  * Name fields descriptively (e.g., *Sales per Customer (INCLUDE)*, *Sales across Regions (EXCLUDE)*).
  * Validate with a simple crosstab before styling; confirm that results reflect the intended grain.

**Bottom line:** LOD expressions provide precise, declarative control over calculation granularity—unlocking robust cohorting, per-entity rollups, and multi-level context that remain correct as views and filters change.
