# Tableau parameters

## What parameters are

* Single-value, user-controlled variables (string, integer, float, boolean, date, datetime) that drive interactivity.
* Values can be free-form (**All**), constrained to a **List**, or restricted to a **Range** (with steps).
* Optional **dynamic refresh** on workbook open: populate current value and/or allowable values from a field.

## Creating parameters

* **From an existing field:** right-click field → *Create ▸ Parameter*. Defaults to field’s data type; allowable list seeded from field values. Can switch to dynamic *When workbook opens* to keep options current.
* **From scratch:** *Data pane ▸ Create Parameter*; set name, type, display format, allowable values, default/current value.
* After creation, parameters must be **wired** into the workbook (calculation, filter, or reference line) and then surfaced via **Show Parameter**.

## Core use patterns

### 1) In calculated fields (logic, highlighting, thresholds)

* Reference directly by name inside calcs (e.g., `SUM([Sales]) > [Threshold %]` or `[Sub-Category] = [Sub-Category Parameter]`).
* Typical outcomes: boolean filters/flags, dynamic color highlights, switchable metrics, scenario inputs.

### 2) As drivers for filters (Top/Bottom N, conditional limits)

* In a dimension filter’s **Top** tab, select **By field** and point the N value to an **integer parameter** (e.g., *Top N*).
* Extends to other conditional filters (e.g., parameterized numeric/date thresholds).

### 3) For dynamic reference lines

* Add a reference line to an axis and set **Value = [Parameter]** (e.g., *Select Date*).
* Combine with a *TODAY()*-based calc to set a dynamic default on open.

## Worked examples (patterns to reuse)

* **String parameter from field:** Build *Sub-Category Parameter* → enable dynamic allowable values from `[Sub-Category]` on open → use `[Sub-Category]=[Sub-Category Parameter]` to color the selected bar.
* **Integer parameter (Top N):** Create *Top N* (1–50, step 1, default 10) → plug into dimension filter’s **Top** tab to control how many items appear.
* **Date parameter:** Create *Select Date* → set *Value when workbook opens* to a calc `TODAY()` → bind to a reference line on a continuous date axis.

## Design and governance guidelines

* **Name with intent** (e.g., `Param_TopN`, `Param_SelectDate`, `Param_SubCat`) and add descriptions for maintainability.
* **Constrain inputs** appropriately (List/Range) to prevent invalid states; use display formats (percent, currency, date) that match business context.
* Prefer **dynamic allowable values** for evolving domains (e.g., members of a dimension) while keeping a sensible default current value.
* **Connect visibly**: always pair parameters with a clear visual response (color, label, filter, line) and expose controls where relevant.
* **Keep logic modular**: separate parameter inputs from downstream calcs (e.g., `Flag_SelectedSubCat`, `TopN_Filter`) for reuse and testing.
* **Combine with LOD/table calcs** thoughtfully: parameters can set benchmarks for FIXED LOD KPIs or choose table-calc modes (e.g., moving window size).
* **Test edge cases**: nulls, empty lists after refresh, out-of-range dates, and interactions with relative date logic.
* **Performance awareness**: parameters themselves are lightweight; cost arises from dependent calcs—favor aggregate-safe expressions and avoid excessive row-level recomputation.

## When to choose a parameter vs. a filter

* **Parameter:** single value that needs to drive logic across multiple sheets, control non-field options (e.g., sort mode, window size), or set a **reference value** (threshold, date, constant).
* **Filter:** subset of data values where multiple selections or direct domain membership control is required.
* Often both are complementary: parameter selects the rule; filter displays the result.
