# Scheduling data updates, alerts, and subscriptions in Tableau Server/Cloud

## Keep data fresh

* **Connection strategy**

  * **Live** connections update continuously; use when latency is low and governance allows direct access.
  * **Extracts** are point-in-time snapshots; prefer for performance, offline use, or constrained sources.
* **Refresh planning**

  * Define required **freshness SLA** (hourly/daily/weekly/monthly) per data source.
  * Choose **full** vs **incremental** refresh; use incremental when a reliable key for new rows exists to reduce load.
  * In **Tableau Server**, refresh windows are admin-defined schedules; in **Tableau Cloud**, creators configure frequency and timing.
  * For on-prem sources with Tableau Cloud, configure **Tableau Bridge** to enable scheduled refreshes.

## Configure extract refreshes

* At publish time (workbook or standalone data source), set:

  * **Frequency & time(s)**; align to upstream ETL completion.
  * **Refresh type** (full/incremental) and **key field** for incremental.
  * **Credentials** (**embed** when unattended refresh is required).
* Post-publish management:

  * Use the **Extract Refreshes** tab on the data source to add, edit, or review schedules.
  * Centralize monitoring and alert on failures; document ownership and escalation.

## Operational tips for extracts

* Minimize extract size (filters, aggregation, hidden unused fields).
* Stagger heavy jobs to avoid resource contention.
* Tag and describe data sources with **refresh cadence** and **last refresh** expectations.
* Test refresh end-to-end before broad distribution.

## Schedule Tableau Prep flows

* Publish **Prep flows** to combine/clean data and **output to published data sources**.
* Create **Scheduled Tasks** (admin-defined schedules on Server; rich presets on Cloud) from the flow’s page.
* Chain flows thoughtfully; ensure dependency order and add status notifications.

## Manage published workbooks with alerts and subscriptions

### Data-driven alerts

* Purpose: notify stakeholders when a **threshold** on a **continuous axis** is crossed after refresh.
* Configuration:

  * Select axis → **Watch → Alerts → Create**.
  * Set **condition** (above/below/equal), **threshold**, **frequency** (once / daily / as often as possible), **recipients** (users/groups), and **visibility** (allow self-subscribe).
* Practices:

  * Place meaningful thresholds (SLOs, KPI targets).
  * Limit alert frequency to reduce noise; include clear **subject lines**.
  * Review alerts when definitions or seasonality change.

### Subscriptions (scheduled emailed snapshots)

* Purpose: distribute **PDF and/or image** exports of a view or complete workbook on a cadence or **on data refresh**.
* Configuration:

  * **Watch → Subscriptions**: choose recipients (users/groups), **scope** (this view vs entire workbook), **format** (PDF/image), **page size/orientation**, **subject/message**, and **schedule**.
* Practices:

  * Prefer sending **single, purpose-built views**; align formatting to recipients’ reading context.
  * Use **“don’t send if view is empty”** to avoid noise on refresh issues.
  * Periodically audit active subscriptions for relevance and duplication.

## Governance & reliability checklist

* **Credentials & security:** embed credentials only for trusted, least-privilege service accounts; rotate regularly.
* **Documentation:** record data lineage, refresh type, schedule, owner, and contact in descriptions/tags.
* **Monitoring:** track refresh success, Bridge health (if used), alert/subscription delivery, and data freshness KPIs.
* **Change control:** coordinate schedule changes with upstream pipelines; communicate impacts to subscribers and alert owners.
* **Performance:** prefer incremental refresh; reduce extracts with filters/aggregation; avoid overlapping heavy flows/refreshes.

By applying these practices, published content remains timely, reliable, and proactively communicated, while platform resources are used efficiently and stakeholders receive actionable updates at the right cadence.
