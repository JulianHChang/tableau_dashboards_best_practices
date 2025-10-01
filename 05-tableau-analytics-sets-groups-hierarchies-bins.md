

## Overview

Tableau offers several ways to structure and segment data—**sets**, **groups**, **hierarchies**, and **bins**—each serving different analytical needs: membership logic, category consolidation, drillable structure, and numeric bucketing.

---

## Sets — boolean membership for dynamic slicing

* **What they are:** Boolean dimensions (In/Out) that behave like TRUE/FALSE flags; icon shows a Venn diagram. Created **only on dimensions**.
* **Creation modes**

  * **Fixed sets:** Select marks in the view (or list values in the UI) → membership is frozen.
  * **Dynamic sets:** Built from a field; membership updates with data (choose **Use all**).
  * **Conditional/Top-N:** Define membership on the **Condition** or **Top** tabs using *By Field* (simple aggregations) or *By Formula* (custom logic). Examples:

    * Avg sales between 50k and 100k.
    * Top 50 customers by SUM(Sales).
* **Usage patterns**

  * As a **filter** (keep In), on **Rows/Columns** (split In vs Out), or on **Color** to highlight membership.
  * In calcs: `IF [Set Name] THEN …` applies logic to members only.
  * **Set Control** (Show Set) enables interactive inclusion/exclusion.
  * **Combined sets** (same base field): union, intersection, or asymmetric combinations (e.g., top 50 sales **AND** negative profit).

**Example applications**

* Exclude fixed outliers via a fixed set.
* Maintain a rolling “Top 50 by Sales” using a dynamic Top-N set.
* Intersect “Top 50 Sales” with “Unprofitable” to spotlight risky accounts.

---

## Groups — manual category consolidation (always dimensions)

* **What they are:** User-defined categorical fields (paperclip icon) that merge multiple member values under new labels; useful for regional rollups, data cleaning (“UK” + “United Kingdom”), and ad-hoc scenarios.
* **Creation & editing**

  * From **view**: multi-select marks → **Group** → a new `[Field] (group)` is created and often placed on Color.
  * From **Data pane**: Create → **Group**; same editor as **Edit Group**.
  * Editor features: rename groups, drag/drop values, **Group/Ungroup**, **Include Other** to catch all non-grouped (including future values).
* **Behavior:** Acts like any discrete dimension—usable for rows/columns, color, filters.

**Example workflow**

* Build “Electronics” from Appliances/Copiers/Machines; create “Furniture” from Chairs/Furnishings/Tables; optionally keep remaining as **Other**.

---

## Hierarchies — drillable field relationships

* **What they are:** Ordered collections of related dimensions (e.g., **Category → Sub-Category → Manufacturer → Product Name**). Enable **drill down/up** via +/− icons in the view.
* **Creation & maintenance**

  * Drag lower-level field onto higher-level field (or use **Hierarchy ▸ Create/Add to Hierarchy**).
  * Reorder by drag/drop; remove via **Remove Hierarchy** (fields remain intact).
* **Behavior in views:** Adding the hierarchy places the top field; clicking **+** progressively adds lower levels. Dates behave like implicit hierarchies in views (Year → Quarter → Month → Day).

**Example**

* Recreate “Product Hierarchy” and drill from Category to Product Name; roll up with **−** to navigate levels.

---

## Bins — numeric bucketing into intervals

* **What they are:** Discrete categories derived from a measure by value ranges (histogram icon). Default behavior: discrete dimension; can be converted to continuous.
* **Creation**

  * Right-click a measure → **Create ▸ Bins**; set **bin size** or use **Suggest Bin Size** (based on `3 + log2(n) * log(n)`).
  * UI shows min, max, range, and distinct row count to guide sizing.
* **Usage**

  * Place the binned field on **Columns/Rows** with a count or distinct count measure to produce histograms or distribution tables.

**Example**

* `Sales (binned by 500)` then CNTD(Customer Name) to see how many customers fall in each sales bucket.

---

## Selection guidance

* **Sets** for rule-based or rank-based membership, dynamic highlighting, and interactive controls; combine sets for multi-condition cohorts.
* **Groups** for curated category consolidation and data cleanup; leverage **Include Other** for completeness and future values.
* **Hierarchies** to support intuitive drill paths from summary to detail; order fields to match analytical flow.
* **Bins** to explore distributions and segment continuous measures into meaningful ranges; tune bin size to analytical resolution.

---

## Governance & usability tips

* Name artifacts for intent (e.g., `Set_Top50_Sales`, `Group_OfficeCategories`, `Hierarchy_Product`).
* Document membership rules in captions or descriptions; expose **Set Control** where interactivity adds value.
* Prefer **dynamic sets** for evolving data; use **fixed sets** for one-off exclusions or snapshot analyses.
* Validate groupings and bin sizes against business meaning (avoid arbitrary thresholds without rationale).


Groups
Groups are denoted by paper clip. It lets you combine several members of a single dimension
into categories that create a new dimension field that did not exist in the original data set. It
can be created from dimensions in a data pane or directly from the visualization.
Sets
Sets are useful for viewing and highlighting data that meets specific criteria. Sets are always
binary it means that you are either in the set or not. Once a set is created, it can be used as a
filter or combine set to show either data that exists in one, but not in the other, or the
combination of all data found in either set. You can combine two sets only if these are created
using same dimension. It can be created from dimensions in a data pane or directly from the
visualization.
Hierarchies
It allows us to organize the dimensions in our data. Usually, we create the hierarchy for related
columns for example – Country, State, City, Zip.
When we use hierarchy in a view, we can drill up and down along the fields of the hierarchy.
Hierarchies can be created in two ways:
· Select multiple dimensions and right click and select Create Hierarchy
· Drag and drop one field to another