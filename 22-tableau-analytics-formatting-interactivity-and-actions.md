# Best-practice summary: Formatting, interactivity, and actions in Tableau

## Color: communicating meaning without clutter

* **Palette types**

  * **Categorical** (discrete): assign distinct colors to categories; ideal for stacked bars and breakdowns. Limit category count to avoid visual noise.
  * **Quantitative** (continuous): encode magnitude with color.

    * **Sequential**: single-hue light→dark for values that are all ≥0 or all ≤0.
    * **Diverging**: two hues split around a meaningful **center** (e.g., 0 or target) for positive/negative ranges.
* **Management and overrides**

  * Override defaults via the **Marks → Color** card: choose palette(s), set **steps**, **reverse**, **use full range**, and define **start/end** and **center** (for diverging).
  * Enter **hex codes** or pick from multiple palettes; manually assign category colors for brand consistency.
  * Dragging a measure (e.g., *Sales*) to **Color** switches to an appropriate continuous palette; still configurable.

## Animations: add clarity—never distraction

* **Scope & access**: **Format → Animations** at workbook or per-worksheet level (sheet can override workbook).
* **Speed**: presets (0.3s fast, 0.5s medium, 1.0s slow, 2.0s very slow) or custom **0.01–10s**.
* **Style**: **Simultaneous** (all marks at once) or **Sequential** (phased).
* **Good to know**

  * Disabled on Internet Explorer; heavy mark counts may auto-disable for performance.
  * Not applied to polygons/maps, text tables, headers, pie charts, trend/forecast lines, or page history marks.
* **Practice**: use sparingly to highlight change (filtering, sorting, adding/removing marks); avoid decorative motion.

## Legends: control visibility and placement

* **Defaults**: created automatically when color/size/shape encodings exist.
* **Placement**: drag legends between worksheet panes (Pages/Filters/Marks) to avoid crowding.
* **Visibility**: hide via legend **⋮ → Hide Card** or **Analysis → Legends**; restore from the same menu.
* **Note**: worksheet legend settings do **not** govern dashboard legend behavior (managed separately on the dashboard).

## Dashboards: foundation and interactive elements

* **Purpose**: combine multiple sheets; enable cross-sheet exploration beyond a single view.
* **Interactive elements location**: on a dashboard, select a sheet → **Analysis** menu shows **Legends, Filters, Highlighters, Parameters** for that sheet.

  * Add/remove legends from **Analysis → Legends**.
  * Add filters/highlighters from **Analysis → Filters/Highlighters** (parameters are global controls, not sheet-bound).

## Dashboard actions: make exploration intuitive

### Filter actions

* **Behavior**: restrict target sheets based on selections in source sheets (e.g., clicking California filters a detail table).
* **Key options**

  * **Name** (supports dynamic text via **Insert**).
  * **Source Sheets** (sheets or dashboard that emit the filter).
  * **Run action on**: **Hover**, **Select** (optionally single-select), or **Menu** (tooltip menu item).
  * **Target Sheets** (one or many; can target another dashboard).
  * **Clearing selection**: **Keep filtered values**, **Show all values**, or **Exclude all values** (blank targets).
  * **Filter** scope: **All fields** or **Selected fields** (must match data types across source/target).

### Highlight actions

* **Behavior**: emphasize matching marks without removing context; dims non-matches.
* **Options**: similar to filter actions; **Target Highlighting** can be **All Fields**, related dates/times, or specific fields.

### URL actions

* **Behavior**: open external content or update a **web page object** on the dashboard.
* **Key options**

  * **URL Target**: open in new tab if no web object exists (default) or always open in a new tab; with a web object present, updates that object.
  * **URL** field: static text, parameters, or fields inserted as `<Field Name>`; compose partial URLs (e.g., `www.site.com/<OrderID>`).
  * **Notes**: `https` is prepended automatically; built-in test link provided.
  * **Advanced**: encode unsupported characters; allow multi-value parameters with custom delimiters.

### Other useful actions (brief)

* **Go to Sheet**: navigate to a sheet or another dashboard via select/hover/menu.
* **Change Parameter**: update parameter values from interactions.
* **Change Set**: add/remove marks to a set via interactions.

## Practical guidance and guardrails

* **Color**

  * Match palette type to data semantics (sequential vs diverging); define a **center** for diverging scales.
  * Limit categorical colors; group long tails into “Other” or redesign.
  * Standardize brand palettes; document hex codes.
* **Animations**

  * Favor **short** durations and **sequential** style for complex updates; validate performance on realistic data volumes.
* **Legends**

  * Show only essential legends; co-locate near the related viz; keep consistent order and naming across the dashboard.
* **Filters vs highlights**

  * **Filter** when focus is required; **highlight** when comparison context matters.
  * Prefer **Menu** triggers for power users; **Select/Hover** for fast exploration; specify **clear behavior** explicitly.
* **URL actions**

  * Sanitize/encode dynamic values; ensure targets respect authentication and security policies.
* **Testing**

  * Verify action behavior with multi-selects, cleared selections, and cross-dashboard targets.
  * Confirm accessibility: color contrast, red/green reliance, keyboard navigation for menus.

**Bottom line:** Use color intentionally, apply animations sparingly, manage legends deliberately, and design actions that mirror expected user journeys. The result is a clear, performant, and genuinely interactive dashboard experience.


# Dashboard and its Action for Interactivity
In Tableau, dashboards are the collection of views that are built with different dimensions and
measures, or even built from different data sources.
To add the actions, navigate to Dashboard menu -> Actions -> Add Actions
1. Highlight Action
2. Filter Action
3. URL Action
Actions can be run on either Select, Hover or Menu.

# Create a story using dashboards or views
A Tableau story is a connected series of worksheets and dashboards, that further allows us to
capture insights and share them in a sequential manner.
A Tableau story consists of one or more story points. One story point contains only one
dashboard or worksheet that highlights specific details.