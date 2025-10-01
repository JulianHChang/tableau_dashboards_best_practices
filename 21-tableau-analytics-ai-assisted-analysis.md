# AI-assisted analysis in Tableau (Tableau Agent & Tableau Pulse)

## Purpose and scope

* Tableau integrates generative AI and analytics to accelerate exploration, explanation, and action.
* Two core components:

  * **Tableau Agent** (formerly Einstein Copilot): conversational analysis and assistive authoring inside Tableau Cloud.
  * **Tableau Pulse**: metric-centric experience that explains performance with natural language insights and guided drill-downs.
* AI operates within Salesforce’s **Einstein Trust Layer** for privacy and security.

---

## Tableau Agent (conversational assistant)

### What it does

* Interprets natural-language prompts to **build and modify visualizations**, **apply filters**, and **author/edit calculations**.
* Understands **schema and semantics** of the connected dataset; suggestions are context-aware.

### Access & setup

* Runs in **Tableau Cloud**; requires premium licensing (e.g., Tableau+).
* Works on data connected in a Cloud workbook (e.g., Superstore.csv).

### Effective usage patterns

* **Kick-start exploration**: use Agent’s *Suggestions* to surface quick “first questions” (e.g., monthly sales trend) and auto-generate vizzes.
* **Iterate by prompt**: refine visuals with natural language (e.g., “split line by state, show top 3 states”). Agent adjusts marks, filters, and context appropriately.
* **Bootstrap calculations**: request draft calcs (e.g., FIXED LOD for distinct customers by state) and then reuse or refine in analysis.

### Quality & governance practices

* Treat outputs as **assistive drafts**—**verify logic** and **validate results** against known totals or simpler views.
* Prefer **transparent, maintainable calcs**; keep Agent-generated expressions aligned with naming standards and documentation.
* Be mindful of **filter semantics** (e.g., context filters vs. regular filters when Agent applies “Top N”).

---

## Tableau Pulse (metric-driven insights)

### What it is

* A layer for defining **reusable metric definitions** (e.g., Sales) and creating multiple **metrics** (e.g., MTD, YTD) on a shared semantic foundation.
* Delivers **natural-language narratives**, **sparklines**, and **diagnostics** (trends, drivers, outliers) for decision-makers.

### Define a robust metric

1. **Definition**

   * **Measure** and **aggregation** (e.g., Sales as SUM).
   * **Time dimension** and **granularity** (e.g., Month); set **date offsets** if data lags.
   * Choose **running total vs. non-cumulative** series appropriately.
   * Add **definition filters** only when the population must be fixed.
2. **Adjustable filters**

   * Expose key slicers (e.g., Region, Category, Item) for user-driven breakdowns and root-cause analysis.
3. **Formatting**

   * Apply clear units (currency, percentage) and consistent number formats.
4. **Insights configuration**

   * Set **favorability** (up is good/neutral/bad) to align narratives with business meaning.
   * Identify **record identifiers/names** (e.g., Order ID) to enable **record-level outlier** insights.
   * Enable relevant insight types:

     * **Trends & change-points** (e.g., a new trend vs prior trend).
     * **Contributions & breakdowns** (top/bottom drivers, concentrated contributions).
     * **Records/transactions** (large or anomalous lines).

### Operate and scale metrics

* **Follow** metrics for personal summaries; invite **followers** for shared awareness.
* Toggle **Overview ↔ Breakdown** to move from KPI to driver analysis using adjustable filters.
* **Customize per role**: set filters (e.g., Central region) and **goals/targets** (e.g., $700k YTD) to personalize narratives and visuals.
* **Distribute in the flow of work**: access on **mobile**, **embed** in Cloud dashboards using the same published data source, and **integrate** with **Slack/Teams**.

---

## Governance & design recommendations

* **Licensing & environment**

  * Plan for Cloud access and premium licensing (Agent); publish data sources for Pulse.
* **Trust & validation**

  * Require **human review** of AI outputs, especially generated calculations and causal explanations.
  * Maintain **data quality checks** and metric audits; version metric definitions.
* **Consistency**

  * Standardize **metric names, descriptions, and favorability rules**; apply common time comparisons (primary/secondary windows).
  * Document **filter intent** (definition vs. adjustable) and **date offsets**.
* **Performance & usability**

  * Keep metric filters focused to reduce noise and improve insight precision.
  * Use **granularity** that reflects decision cadence (e.g., weekly for volatile daily data).
* **Change management**

  * Start with a **small set of high-value metrics** (e.g., Sales, Pipeline, Tickets) and expand based on adoption.
  * Provide **prompt libraries** and **calculation patterns** to accelerate reliable Agent interactions.

---

### Bottom line

* **Tableau Agent** accelerates authoring and exploration via natural language—best used for ideation, iteration, and code scaffolding with disciplined validation.
* **Tableau Pulse** operationalizes KPI monitoring through a shared metric layer, automated insights, and accessible narratives—best used to align teams on performance, drivers, and next actions.
