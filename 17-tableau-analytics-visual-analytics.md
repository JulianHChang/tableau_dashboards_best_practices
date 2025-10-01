# Visual analytics in Tableau (trends, clustering, distributions, forecasting)

## Purpose

Leverage Tableau’s built-in statistical features—trend lines, clustering, distribution visuals, and forecasting—to deepen analysis beyond basic charts, surface outliers, explain patterns, and project future values. Integrations with R/Python extend capabilities when needed.

---

## Trends (time and continuous relationships)

* **When to use**: Quantify direction/strength of change over time or correlation between measures.
* **Setup**:

  * Use continuous fields on both axes (exception: a discrete date may define headers while the other axis remains continuous).
  * Discrete fields on Color/Rows/Columns create **separate trend lines** (factors).
* **Refining scope**:

  * Compute trends for **selected marks** or segment periods via a discrete “Period” field; optionally set **independent axis ranges** per segment.
* **Model choice**:

  * **Linear**: constant rate of change.
  * **Logarithmic**: diminishing returns.
  * **Exponential**: accelerating growth (use cautiously).
  * **Power**: non-linear, size^k relationships.
  * **Polynomial (2–8)**: S-shaped/complex curves.
  * Only force **y-intercept = 0** when the phenomenon requires it.
* **Diagnostics**:

  * Inspect **equation, R², P-value** (Describe Trend Model; tooltips).
  * High R² improves fit; **P ≤ .05** suggests significance.
  * Compare subset lines vs overall by toggling “line per color” and factor fields.

### Model validation

* Export **predictions and residuals** (Worksheet ▸ Export ▸ Data) to visualize residual scatter (even spread around zero indicates adequate model).
* Use dashboard highlight actions to relate residual outliers back to source marks.

---

## Data Guide (explain and audit)

* Access the **signpost** pane to see:

  * Viz description, applied filters, fields in use, mark counts.
  * **Detected Outliers** with suggested contributing factors.
* Apply for quick root-cause leads and **data quality checks** (e.g., duplicates causing inflated sums).

---

## Clustering (k-means)

* **Goal**: Group similar marks using chosen variables; Tableau selects **k** by default, adjustable by the analyst.
* **Workflow**:

  * Add **Cluster** from Analytics pane; include/exclude variables to reshape groups.
  * **Materialize clusters as Groups** by dragging to the Data pane for reuse, actions, aliases, and calculations (use **Refit** to recompute).
* **Use cases**: Segmentation (customers, properties, patients), geographic patterning when mapped.

---

## Distributions (reference analytics)

* Add **bands/lines/box plots** per **table/pane/cell** for:

  * **Standard deviations**, **confidence intervals**, **percentiles/quantiles**, **averages**.
* Practical patterns:

  * Per-pane standard deviation bands to compare categories.
  * Dual-axis distributions on scatterplots to flag **multivariate outliers** (inside/outside 1σ on both axes).

---

## Forecasting (time-series)

* **Requirements**: Date (or reconstructable date parts) or integer sequence; relational sources (not OLAP); ≥5 data points; avoid table calcs on forecasted measures.
* **Configuration**:

  * Forecast length (automatic or custom); show **prediction intervals**.
  * **Source data grain** can be finer than view (e.g., model on monthly data, display yearly).
  * Optionally **exclude last period** to avoid partial-period bias.
  * Model types: automatic with/without seasonality, or **custom** (additive/multiplicative trend and seasonality).
* **Interpretation**:

  * Seasonality controls cyclic fluctuations; multiplicative components yield proportionally larger peaks/troughs at higher levels.
  * **Describe Forecast** for summary, diagnostics, and model details.

---

## Practical guidance checklist

* Match model to phenomenon; prefer simplest adequate model.
* Segment trends thoughtfully (periods, cohorts) and verify with diagnostics.
* Use Data Guide early for outlier explanation and data issues.
* Convert meaningful clusters to reusable **Groups** with clear aliases.
* Combine distributions with averages to anchor interpretation; use per-pane scope for fair comparisons.
* Validate forecasts against known seasonality; confirm data grain and completeness.
* Keep visuals readable: minimal overlapping lines, clear legends, and concise annotations highlighting exceptions, not restating the obvious.

Applying these practices elevates visuals into robust analytical tools that explain patterns, surface anomalies, and inform forward-looking decisions.


# Reference Lines and Trend Lines
Reference lines marks a specific value on an axis. We can only create a reference lines from the
measures currently in a view.
There are two ways to create reference lines:
· Right click on axis and select Reference line
· Using Analytics Tab
Trend Lines can be used to explore the relationships between two measures in your data. It can
be added from Analytics tab.
Trend Lines could be:
· Linear
· Logarithmic
· Exponential
· Polynomial
· Power
R-Squared is a statistical measure of how well the trend fits the data. Value of 1 or 100% is a
perfect fit.
P-value is a probability value associated with significance. Smaller is better, ideally less than .05