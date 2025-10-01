# Date calculations and type conversions in Tableau

## Organize and extract date parts

* Use **DATEPART(part, date, [startOfWeek])** to return integer parts such as `year`, `quarter`, `month`, `week`, `day`.

  * Example: `DATEPART("year", #2024-01-13#) → 2024`.
  * `startOfWeek` (`"sunday"`/`"monday"`) affects week results.
* Use **DATENAME(part, date, [startOfWeek])** for the **text** name of a part (e.g., `"January"`).
* Shorthand functions return integers directly: **YEAR**, **QUARTER**, **MONTH**, **WEEK**, **DAY**.
* ISO week-based variants: **ISOYEAR**, **ISOQUARTER**, **ISOWEEK**, **ISOWEEKDAY**.
* Practical pattern: place `Order Date` as discrete, then add `YEAR([Order Date])`, `MONTH([Order Date])`, `DAY([Order Date])` (or `DATEPART`) to display parsed components.

## Manipulate dates for analysis

* **DATETRUNC(part, date, [startOfWeek])**: truncate/aggregate to a higher grain (e.g., month-start).

  * Example: `DATETRUNC("month", #2024-01-13#) → #2024-01-01#`.
* **DATEADD(part, n, date)**: shift dates forward/backward (negative `n` for subtract).

  * Example: `DATEADD("month", 5, #2024-01-13#) → #2024-06-13#`.
* **DATEDIFF(part, date1, date2, [startOfWeek])**: difference between two dates at a chosen grain.

  * Example: `DATEDIFF("year", #2024-01-13#, #2025-01-13#) → 1`.
* Dynamic reference points: **TODAY()** (date) and **NOW()** (datetime), commonly paired with `DATEDIFF` for age/recency metrics.

## Convert types to enable functions

* **STR(value)**: cast to string (useful for IDs or labeling).

  * Example: `STR([Discount] * 100) + "%"`.
* **INT(value)** / **FLOAT(value)**: cast to integer/decimal.

  * Example: `INT("5.2") → 5`, `FLOAT("5.2") → 5.2`.
* **DATE(value)** / **DATETIME(value)**: cast recognized patterns to date/datetime.

  * Examples: `DATE("13/01/2024") → #2024-01-13#`, `DATETIME("January 13, 2024 12:00:00")`.
* **DATEPARSE(format, text)**: parse non-standard strings with an explicit pattern.

  * Example: `DATEPARSE("yyyyMMdd", "20240113") → #2024-01-13#`.
* Construct dates/times explicitly:

  * **MAKEDATE(year, month, day)** → date (`MAKEDATE(2024,1,13)`).
  * **MAKETIME(hour, minute, second)** → time in a datetime (`#1899-01-01 16:52:34#`).
  * **MAKEDATETIME(date, time)** → combine a date with a time.

## Representative application patterns

* **Extract calendar components**: add `YEAR/MONTH/DAY` fields alongside discrete `Order Date` to analyze seasonality.
* **Normalize to periods**: use `DATETRUNC("month",[Order Date])` for period-based KPIs (e.g., MTD/YTD rollups).
* **Shift windows**: `DATEADD("day", 5, [Order Date])` for offset comparisons; pair with `DATETRUNC` for aligned windows.
* **Recency and age**: `DATEDIFF("year",[Order Date], TODAY())` for age in years (dynamic with today’s date).
* **Derive dates from IDs**: `MAKEDATE(INT(SPLIT([Order ID],"-",2)), 1, 1)` to build a canonical year date (Jan 1) from encoded identifiers.
* **Create labeled dimensions**: `STR([Discount]*100) + "%"`, producing display-ready categories while keeping numeric measures separate for computation.

## Implementation guidelines

* Pick the **appropriate grain**: extract date parts when categorical analysis is needed; truncate with `DATETRUNC` when aligning to calendar periods.
* Keep **calculations explicit and typed**: cast inputs (`STR`, `INT`, `DATE`) so subsequent functions behave as intended.
* Favor **aggregate-safe patterns** when building ratios or period KPIs; combine with Fixed LODs when a stable grain is required.
* Use **dynamic anchors** (`TODAY`, `NOW`) thoughtfully, recognizing that outputs change as time advances.
