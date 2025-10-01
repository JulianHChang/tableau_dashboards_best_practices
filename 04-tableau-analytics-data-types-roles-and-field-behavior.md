# Tableau data types, roles, and field behavior

## Plan analysis before import

* Verify data completeness and cleanliness (for example, full quarter coverage, consistent product names).
* Identify required transformations (for example, `Total Sales = Unit Price × Quantity`).
* Determine whether a single source suffices or if multiple sources must be combined (joins/unions in Desktop; flows/steps in Prep).

## Understand Tableau data types and conversion

* Tableau fields carry a **data type** (e.g., String, Number, Date, Date & Time, Boolean, Geographic) that governs available behaviors.
* **Data type changes** alter functionality:

  * Example: Converting `Order Date` from **Date** to **String** removes the built-in date hierarchy (Year → Quarter → …) and lists each date as text.
* Change data types via the field’s **data type icon** in the Data pane.

## Dimensions vs. measures (semantic role)

* **Dimensions**: qualitative, categorical breakouts (e.g., Country, Product, Customer ID).
* **Measures**: quantitative, aggregated metrics (e.g., Sales, Profit, Quantity).
* Defaults and conversions:

  * Strings/Booleans/Geographics default to **dimensions**. If converted to measures, strings/geographics aggregate to **COUNT (Distinct)**; Booleans cannot be measures.
  * Numbers default to **measures**, but identifiers (e.g., numeric IDs) often function better as **dimensions** for detail and grouping.
  * Dates default to **dimensions** (typically at **Year** initially) and can be aggregated in the view.

**Practical effect**

* As a **measure**, `Order ID` displays a single bar for **COUNT (Distinct)**.
* As a **dimension**, `Order ID` lists each ID as a discrete header.

## Discrete vs. continuous (display role)

* **Color meaning in Tableau pills/icons**: **Blue = Discrete**, **Green = Continuous** (not dimension vs. measure).
* **Discrete** values create **headers** (rows/columns); **Continuous** values create **axes**.
* Defaults:

  * Non-numeric fields → typically **discrete**.
  * Numeric aggregations → typically **continuous**.
* Any combination is possible:

  * A **dimension** can be **continuous** (e.g., Date as a continuous timeline).
  * A **measure** can be **discrete** (e.g., Price bucketed as distinct values).
  * Strings/Booleans are **discrete dimensions only**.

**In-view behavior examples**

* `Sales` as a **continuous measure** → axis with an aggregated value (e.g., SUM(Sales)).
* Switch `Sales` to **discrete (measure)** → a single header for the aggregated value.
* Convert `Sales` to a **discrete dimension** → a header for every distinct sales value.
* Convert `Sales` to a **continuous dimension** → an axis with individual marks (e.g., Gantt-style lines) per value.

## Color legends: continuous vs. discrete

* **Continuous legend** (measure or dimension set to continuous): gradient scale from minimum to maximum, suitable for ranges.
* **Discrete legend**: categorical palette assigning a distinct color per value; palettes recycle colors if categories exceed palette size.

## Implementation guidance

* **Choose the right data type first** to unlock appropriate behaviors (date hierarchies, numeric aggregation, geographic features).
* **Assign the correct role** (dimension vs. measure) to reflect analytical intent: identifiers as dimensions; metrics as measures.
* **Set display form** (discrete vs. continuous) to match how the field should appear: headers for categories, axes for ranges.
* **Validate impact with quick toggles** in the view:

  * Right-click a pill to switch **Discrete/Continuous**.
  * Convert **Measure/Dimension** to confirm expected aggregation or listing.
* **Combine sources thoughtfully**: use Desktop joins/unions for ad-hoc modeling and Prep flows for repeatable cleansing/reshaping.



# Common Data Types in Tableau
· Number
· String
· Geographic
· Date
· Date Time
· Boolean
Important Concepts
Dimensions – The Qualitative fields that describe categories of data. These are the independent
variables. In case we have multiple dimensions in a row or column, the first dimension creates
the pane.
Measures – The Quantitative or numerical fields that measure categories of data. These are the
dependent variables.
Measure Name – It is a dimension that contains a label for each measure in the data source.
Measure Value – It is a measure that contains numerical values of each measure in the data
source.
Latitude and Longitude – These are the Tableau generated geo fields, and it is generated when
we have geographic fields in our data source.

Discrete fields 
Create headers/labels 
Can be sorted 
Always represented in Blue Color 

Continuous Fields
Create Axis
Can’t be sorted
Always represented in Green Color


Aggregation and granularity
By default, measures are aggregated, and the default aggregation is SUM.
Dimensions break down the aggregated total into smaller totals by category or we can say that
dimension provides the granularity in a chart.

Aggregating the Dimensions
You can aggregate a dimension in the view as Minimum, Maximum, Count or Count
(Distinct). When you aggregate a dimension, you create a new temporary measure column, so
the dimension actually takes on the characteristics of a measure.
The only exception in this is if you have numeric dimension let's suppose EXAM ID, in this case
you will find all the aggregate functions similar as measures.

Ratio Calculations
SUM(Profit)/SUM(Sales) sums the profits and sales to whatever the granularity of the view is,
then computes the ratio at that aggregation. Profit/Sales computes the profit ratio at the
lowest level of granularity then sums the ratios to the requested aggregation of the view.

