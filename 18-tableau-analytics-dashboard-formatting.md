# Tableau dashboard formatting

## Purpose and scope

Effective formatting makes dashboards clear, scannable, and decision-ready. Core levers include color, typography, shapes, alignment, tooltips, annotations, padding/layout, lines/banding, and custom palettes/shapes.

## Color

* **Palette types**

  * **Categorical** for discrete fields; assign distinct colors per category.
  * **Quantitative** for continuous fields:

    * **Sequential** (single-hue gradient) for all-positive or all-negative ranges.
    * **Diverging** (two-hue) when values cross a meaningful midpoint (e.g., 0).
* **Controls & options**

  * Adjust palette, **stepped colors** (bucketed ranges), **opacity** (surface overplotting), **borders/halos** (separate dense marks or improve map readability).
* **Practice**

  * Use few, high-contrast hues; reserve accent color for KPIs; ensure accessibility (contrast and color-blind safety).

## Typography (Format ▸ Font)

* Areas: **Pane** (in-chart labels), **Headers** (axes/category labels), **Tooltip** (default text), **Title**, **Totals/Grand Totals**.
* Practice: establish hierarchy (title > headers > labels), limit fonts to one family with size/weight variations; keep numeric labels legible and consistent.

## Alignment (Format ▸ Alignment)

* Controls: **Horizontal**, **Vertical**, **Direction** (text rotation), **Wrap**.
* Practice: align numbers right, categories left; rotate only when essential; prefer wrapping to truncation for key labels.

## Shapes (Marks ▸ Shape)

* Map discrete categories to shapes; avoid for measures (forces discretization).
* Use sparingly; ensure shapes are distinguishable at small sizes and across backgrounds.

## Custom shapes & color palettes

* **Shapes:** add PNG/SVG (transparent backgrounds) to *Documents/Tableau Repository/Shapes*, then reload.
* **Palettes:** edit `Preferences.tps` with `<color-palette name="" type="regular|ordered-sequential|ordered-diverging">` and hex colors; keep ≤20 colors per palette; restart Tableau.
* Practice: codify brand palettes and a minimal “analysis palette” for consistent use.

## Annotations

* Types: **Mark**, **Point**, **Area**.
* Capabilities: editable text, dynamic inserts, movable callouts, configurable lines/arrows.
* Practice: annotate only exceptions, inflection points, or definitions; keep concise and non-overlapping.

## Tooltips

* Fully customizable via Marks ▸ **Tooltip** or **Worksheet ▸ Tooltip**; support dynamic fields and optional command buttons (Keep Only, Exclude, View Data).
* Practice: lead with the mark’s primary metric and context; format numbers/dates; remove noise; keep within 2–4 lines when possible.

## Padding & layout (Layout pane)

* **Outer padding:** space outside the sheet border; separates sheets/objects.
* **Inner padding:** space between border and marks; improves breathing room.
* Practice: apply consistent spacing grid; pair with borders sparingly for structure; avoid crowded edges.

## Gridlines, banding, and shading (Format ▸ Lines/Shading)

* **Gridlines:** adjust color, style, opacity to aid reading without clutter.
* **Shading:** apply header/background colors for emphasis.
* **Row/column banding:** set **Pane/Header** color, **Band Size**, and **Level** (by category or row).
* Practice: prefer light, low-contrast guides; band dense tables to improve row tracking; disable nonessential lines.

---

### Implementation checklist

* Define a **visual style guide** (palettes, fonts, sizes, spacing).
* Use **categorical vs. quantitative** palettes appropriately; limit accents.
* Ensure **readability** (contrast, font size, label density).
* Standardize **padding and alignment** across worksheets.
* Add **meaningful annotations/tooltips**; remove defaults that distract.
* Validate with **color-blind simulators** and print/PDF previews when needed.

Consistent application of these practices produces dashboards that communicate clearly, reduce cognitive load, and foreground the most important insights.
