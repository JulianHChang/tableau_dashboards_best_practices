# Live vs. Extracted Data in Tableau

## Choose refresh strategy based on data behavior

* Decide between **Live** and **Extract** connections by evaluating source update frequency, data volume, expected latency, and authoring/consumption performance needs.
* Certification topics commonly test **when to use each** and **configuration options**.

## Live data — when freshness dominates

* **Definition:** Queries the source on demand; every action (add field/filter/format) triggers a source query.
* **Pros:** Always current; minimal setup choices.
* **Cons:** Can be slow for large datasets or high-latency networks; authoring can feel sluggish.
* **Security setup:**

  * Consider **embedded credentials** vs. requiring viewers to supply their own.
  * Prefer **service accounts** for stability and separation from personal accounts; avoids storing personal creds on Server/Cloud and survives staff turnover.
* **Use when:** Data must be real-time and the source is performant/reliable.

## Extracts — when performance and stability matter

* **Definition:** **Local copies** of selected data, refreshed on a schedule.
* **Core benefits:** Faster authoring and viewing, reduced network dependency, **offline** build capability, and added calculation flexibility.

### Extract configuration options

1. **Storage grain**

   * **Physical tables:** Preserve original table structures at row level; recommended when joining tables to improve performance and reduce extract size.
   * **Logical table:** Materialize joins from the physical layer into one wide table; enables logic at extract time (aggregations, filters, hide fields).

2. **Extract filters (highest level in filter hierarchy)**

   * Remove rows **before** writing the extract to shrink size and improve speed.

3. **Aggregation at extract time**

   * Roll up data to the needed level (e.g., store sales → **state** totals) to cut rows and accelerate downstream work.

4. **Number of rows**

   * Applied **after** filters/aggregation. Options include:

     * **All rows**
     * **Top N** (by a field)
     * **Sample of N rows**
     * **Only updated rows** (requires a field that identifies new records)
   * **Incremental refresh** requires pulling **all rows** initially and a reliable key/date to detect new rows.

5. **History & hygiene**

   * **History:** Track last extract time to assess data freshness during development.
   * **Hide All Unused Fields:** Keep all data but hide fields not used in visuals to declutter the Data pane; unhide as needed.

### Operational notes

* After creation, Tableau switches the connection icon from **single cylinder (live)** to **double cylinder (extract)** to indicate the workbook is reading from the extract files.
* Extract files are stored in the **Tableau Repository** and are the source for visualization rendering until the next refresh.

## Practical selection guidance

* **Prefer Live** for low-latency sources that must reflect real-time changes and where governance requires direct source control.
* **Prefer Extracts** for large/complex data, constrained networks, or heavy authoring where speed and stability are critical. Combine with **filters**, **aggregation**, and **field hiding** to minimize size and maximize performance.



# Live Connection 
Queries the data from the database

Data updates automatically

Workbook performance is slow, if we have large 
amount of data.

Always connected to the data source for the real time updates

If connected to a database, user need to enter the credentials 

# Extract 
Queries the data from the Tableau Data Engine

Manually refresh the extract.

Performance is fast but to refresh the extract is time consuming

Accessed offline
 
No need to enter the credentials 
