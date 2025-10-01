# Publishing, managing, and exporting Tableau content (Server & Cloud)

## Platform concepts

* **Server vs Cloud:** Functionally equivalent sharing platforms; Cloud is hosted by Tableau, Server is self-hosted.
* **Objects & organization:** Content lives in **projects** (folder-like). Supported objects: **workbooks**, **published data sources**, **Prep flows**.
* **Roles & access:** Creators/Explorers publish; Explorers/Viewers consume. Manage access via **groups** and **content-level permissions**; inherit from project by default.

## Keep data current and users informed

* **Refreshes:** Schedule extract refreshes for published data sources; schedule **Prep flows** to produce up-to-date outputs.
* **Connectivity:** For on-prem data with Tableau Cloud, configure **Tableau Bridge** to enable refresh.
* **Notifications:** Configure **data-driven alerts** on thresholds and **subscriptions** for scheduled snapshots.

## Publish workbooks (Tableau Desktop → Server/Cloud)

1. **Sign in** to the correct site/URL.
2. **Publish Workbook** → choose **project**, **name**, **description**, **tags**.
3. **Sheets to show:** Publish all or **limit to selected** (e.g., *Only Dashboards*).
4. **Permissions:** Inherit from project or customize for users/groups.
5. **Data sources handling:**

   * **Embed** local files in the workbook **or**
   * **Publish data source separately** and connect the workbook to it.
   * For extracts that must refresh: **Embed password** and **Allow refresh access**.
   * For external DBs: choose **Prompt** vs **Embedded credentials**.
6. **More Options:**

   * **Show sheets as tabs** (multi-sheet navigation)
   * **Show selections** (preserve marked points)
   * **Include external files** (package Excel/text/images)

> Guideline: Prefer publishing shared data sources for “single source of truth”; keep workbook publication scoped to curated dashboards and clear metadata.

## Publish data sources

* **Publish Data Source** → choose **project**, **name**, **description/tags**.
* **Permissions:** Inherit or customize.
* **Refresh & access:**

  * For **extracts**, set **refresh schedules** (Bridge for on-prem with Cloud).
  * For **live/DB** sources, choose authentication model (**embedded** vs **prompt**).
* **Post-publish wiring:** Option **Update workbook to use the published data source** to re-point existing workbooks and reduce duplication.

> Governance tip: Centralize business-critical calculations and definitions in published data sources; document fields and certify when applicable.

## Export and distribute content (static and data)

* **PDF:** *File → Print to PDF*; choose **whole workbook / active / selected sheets**, paper size/orientation, and whether to show selections. Control layout per sheet via **Page Setup** (titles, legends, fit).
* **PowerPoint:** *File → Export as PowerPoint* (current view or specific sheets).
* **Images:**

  * **Copy image** to clipboard (*Worksheet → Copy → Image*)
  * **Export image** to file (*Worksheet → Export → Image*)
* **Data out:**

  * **Copy → Data** (selected/all marks)
  * **Copy/Export → Crosstab to Excel** (opens a cross-tabbed Excel file)
  * **Export → Data** (to Access DB)

> Communication tip: Prefer interactive dashboards for exploration; provide PDFs/PowerPoints for executive briefings or archival.

## Operational and design recommendations

* **Projects & permissions:** Use a clear project hierarchy (e.g., *Development → QA → Production*); apply group-based permissions and minimize item-level exceptions.
* **Naming & metadata:** Use descriptive names, tags, and descriptions; include business purpose and refresh cadence.
* **Credentials & security:** Embed only when necessary; favor least-privilege access; audit who can download full data.
* **Performance:** Publish extracts sized for purpose; filter upstream (data source filters); avoid heavy per-user custom queries.
* **User experience:** Limit exposed sheets to finalized dashboards; enable tabs only when helpful; ensure parameters/filters are intuitive.
* **Change management:** Version content via new projects or naming conventions; deprecate and archive retired assets.

By following these practices, content is organized, secure, refreshable, and easy for audiences to find, trust, and reuse across the organization.
