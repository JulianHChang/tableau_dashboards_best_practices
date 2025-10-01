# String calculated fields in Tableau

## Purpose and scope

* Enable parsing, cleaning, searching, case standardization, and advanced manipulation of text directly in Tableau calculated fields.
* Support both simple positional extraction and dynamic pattern logic (including **regex**).

## Core categories and how to apply them

### 1) Positional extraction & whitespace handling

* **RIGHT(str, n)** / **LEFT(str, n)**: keep last/first *n* characters.
  *Ex.* `RIGHT("Example",5) → "ample"`, `LEFT("Example",4) → "Exam"`.
* **MID(str, start, [len])**: substring from a starting index with optional length.
  *Ex.* `MID("Example",3) → "ample"`, `MID("Example",3,3) → "amp"`.
* **TRIM(str)**, **LTRIM(str)**, **RTRIM(str)**: remove surrounding/leading/trailing spaces.

**Practice tip:** Use LEFT/MID/RIGHT together to decompose structured IDs (e.g., country code, year, sequence) and TRIM variants to sanitize imported text.

### 2) Substring search, tests, and replacement

* **CONTAINS(str, sub)** → TRUE/FALSE if `sub` exists.
* **STARTSWITH(str, sub)** / **ENDSWITH(str, sub)** → position-specific checks (ignores leading/trailing whitespace).
* **REPLACE(str, sub, repl)**: exact-match substitution (case-sensitive).
* **FIND(str, sub, [start])**: index of first occurrence (1-based; 0 if not found).
* **FINDNTH(str, sub, n)**: index of the *n*th occurrence.

**Practice tip:** Use CONTAINS/STARTSWITH for quick categorization flags; use REPLACE for standardized labels; use FIND/FINDNTH for position-aware parsing when delimiters repeat.

### 3) Case normalization

* **UPPER(str)**, **LOWER(str)**, **PROPER(str)** (title case with punctuation/space as word breaks).
  *Ex.* `PROPER("jim o'connell") → "Jim O'Connell"`.

**Practice tip:** Normalize case before joins or grouping to avoid duplicate categories caused by inconsistent capitalization.

### 4) Splitting, measurement, spacing, and ASCII utilities

* **SPLIT(str, delim, partIndex)**: token by delimiter; supports negative indices from the right.
  *Ex.* `SPLIT("Ex-am-ple","-",1) → "Ex"`, `SPLIT("Ex-am-ple","-", -2) → "am"`.
* **LEN(str)**: character count (useful for validation, e.g., 4-digit years).
* **SPACE(n)**: n spaces (formatting or padding).
* **CHAR(code)** ↔ **ASCII(str)**: convert between characters and ASCII codes.
  *Ex.* `CHAR(72) → "H"`, `ASCII("H") → 72`.

**Practice tip:** Validate parsed fields with LEN (e.g., ensure year length = 4); use SPLIT for delimiter-driven schemas (IDs like `CCYY-XXXX`).

### 5) Advanced manipulation

* **Regex functions** (mentioned in scope) enable pattern matching and complex extracts when fixed positions vary or delimiters are inconsistent.

## Representative applications (from examples)

* Decompose `Order ID` into components with **LEFT**, **MID**, **RIGHT** (country code, year, sequence).
* Flag categories using **CONTAINS("er")** or **STARTSWITH("A")**; standardize labels with **REPLACE**.
* Standardize customer names using **UPPER/LOWER/PROPER**.
* Extract the year from `Order ID` via **SPLIT** on `"-"` and validate with **LEN** = 4.

## Implementation guidelines

* **Choose the simplest tool first**: positional functions for fixed formats; **SPLIT** for clean delimiters; **regex** for variable patterns.
* **Normalize before grouping/joining**: apply **TRIM** and **UPPER/LOWER/PROPER** to reduce duplication.
* **Validate parsed outputs**: use **LEN**, **FIND/FINDNTH** checks to confirm expected structure.
* **Be explicit about case sensitivity**: **REPLACE** and some searches are case-sensitive; standardize case prior to comparison.
* **Keep calculations modular and named for intent**: e.g., `OrderID_Year`, `OrderID_Country`, `SubCat_Has_er` for easier reuse and auditing.
