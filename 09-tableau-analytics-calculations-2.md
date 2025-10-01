# Fixed LODs and numeric functions in Tableau

## Fixed Level of Detail (LOD) calculations

* **Purpose**: Lock an aggregate to a declared dimensional grain, independent of the view’s current level of detail.
* **Syntax**: `{ FIXED [Dim1], [Dim2], … : AGG([Field]) }`.

  * Omit dimensions to fix across the entire data set: `{ FIXED : SUM([Sales]) }`.
* **Key behaviors**

  * Always requires an **aggregation** (`SUM`, `AVG`, `MIN`, `MAX`, etc.); can aggregate non-measure fields (e.g., `MAX([Order Date])` per customer).
  * Returns stable values regardless of added or removed view dimensions.

    * Examples:

      * `{ FIXED [Category] : SUM([Sales]) }` → total sales per category.
      * `{ FIXED [Category], [Sub-Category] : SUM([Sales]) }` → total by category–sub-category pair.
* **Practical use**

  * Use as reference totals, “first/last” at a business grain, or benchmarks that must not fluctuate with the view.

## Numeric calculated fields (transformations and formatting)

### Transformational math

* **Goal**: Derive new values from numeric inputs for analysis or feature engineering.
* **Functions & examples**

  * `DIV(num1, num2)` → integer division (e.g., `DIV(25, 5) = 5`); use for ratios like profit ÷ sales.
  * `POWER(num1, num2)` (e.g., `POWER(10,2) = 100`), `SQUARE(num)` (`SQUARE(3)=9`), `SQRT(num)` (`SQRT(9)=3`).
  * Logs/exponentials: `LOG(num, [base])` (`LOG(25,5)=2`), `LN(num)`, `EXP(num)`.

**Example pattern**

* Create `SQUARE` as `SQUARE(2)` → 4; then `SQRT([SQUARE])` → 2. Demonstrates chaining/nesting of functions.

### Number formatting & normalization

* **Goal**: Standardize display/scale and handle edge cases.
* **Functions & examples**

  * `ROUND(num, digits)` (`ROUND(2.123,2)=2.12`)
  * `FLOOR(num)` (`FLOOR(2.9)=2`), `CEILING(num)` (`CEILING(2.1)=3`)
  * `ABS(num)` (`ABS(-3)=3`) for positive-only magnitudes
  * `SIGN(num)` → −1 | 0 | 1 (e.g., `SIGN(-87)=-1`)
  * `ZN(num)` → replaces null with 0 (e.g., `ZN([Dummy])` turns `null` into `0`)

**Example pattern**

* Display raw `Profit` as a list; add `ROUND([Profit], 2)` to show two decimals; add `ABS([Profit])` to show magnitudes without sign.

## Trigonometric functions (for circular/angle-based logic)

* **Core functions**

  * Constants/conversions: `PI()`, `RADIANS(deg)`, `DEGREES(rad)`
  * Trig and inverses: `SIN`, `ASIN`, `COS`, `ACOS`, `TAN`, `ATAN`, `ATAN2(y, x)`, `COT`
* **Example pattern**

  * `SIN(RADIANS(30))` → 0.5; nesting confirms multiple functions can be combined in one calculated field.

## Implementation guidelines

* **Select the correct grain**: Use **FIXED LODs** to anchor totals or dates to business keys (e.g., category, customer) so results remain stable as the view changes.
* **Aggregate intentionally**: For ratios and rates, prefer aggregate formulations (e.g., `SUM(Profit)/SUM(Sales)`) to avoid row-level bias.
* **Chain functions deliberately**: Combine transformations (e.g., `SQUARE` then `SQRT`) to build readable, modular calculations.
* **Normalize and format**: Apply `ROUND`, `ABS`, `ZN`, etc., to produce consistent, analysis-ready outputs.
