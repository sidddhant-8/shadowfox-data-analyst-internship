# Power BI Build Guide — IBM HR Attrition Dashboard
### ShadowFox Data Analyst Internship — Advanced Level

**Dataset:** `HR_Attrition_PowerBI_Ready.csv` (1,470 employees, 44 columns, cleaned and pre-labeled)
**Tool:** Power BI Desktop
**Estimated build time:** 30–45 minutes following this guide

---

## Before you start

### Step 1 — Import the data
1. Open Power BI Desktop → **Get Data** → **Text/CSV**
2. Select `HR_Attrition_PowerBI_Ready.csv`
3. Power BI will auto-detect types. Check these manually before loading:
   - `EmployeeNumber` → set to **Text** (it's an ID, not a number to sum)
   - `AttritionFlag` → confirm **Whole Number** (this is your 0/1 flag for measures)
   - `AgeGroup`, `TenureGroup`, `IncomeBand` → these are already ordered categories; Power BI may load them as text, which is fine
4. Click **Load** (not Transform — the file is already clean)

### Step 2 — Create all DAX measures
Open `DAX_Measures.md` and create every measure listed there now, before building
any visuals. Building measures first means every visual you add afterward just
*picks* from a ready list instead of you writing DAX under time pressure.

### Step 3 — Set data categories (for map visuals, if used)
Not applicable to this dataset — there's no geography field. Skip.

---

## Dashboard structure: 3 pages

| Page | Purpose | Audience question it answers |
|---|---|---|
| 1. Executive Overview | Headline numbers, at-a-glance health | "How big is the attrition problem?" |
| 2. Attrition Drivers | Root-cause drill-down | "Why are people leaving?" |
| 3. Compensation & Demographics | Pay and workforce composition | "Is pay or demographics part of the story?" |

A consistent **filter panel** (Department, JobRole, Gender, AgeGroup) sits on the
left of every page, built once and synced across all three — see Step 7.

---

## Page 1: Executive Overview

**Layout (top to bottom):**

**Row 1 — KPI cards (4 across the top):**
| Card | Measure | Format |
|---|---|---|
| Total Headcount | `[Total Headcount]` | 1,470 |
| Attrition Count | `[Attrition Count]` | 237 |
| Attrition Rate | `[Attrition Rate]` | 16.12% |
| Avg Tenure | `[Avg Tenure (Years)]` | 7.0 yrs |

Use the **Card** visual (not Multi-row card) for each — one big number, clean and
executive-readable. Add a thin colored top border to each card (red-orange for
Attrition Rate, since it's the metric everyone's watching).

**Row 2 — Two visuals side by side:**

*Left: Attrition Rate by Department* (Clustered Bar Chart)
- Axis: `Department`
- Values: `[Attrition Rate]`
- Sort descending. Sales (20.6%) should be on top, Human Resources (19.0%) second.

*Right: Headcount Composition* (Donut Chart)
- Legend: `Department`
- Values: `[Total Headcount]`
- Shows R&D is by far the largest department (961 of 1,470) — useful context: R&D
  has the *lowest* attrition rate (13.8%) despite being the biggest team.

**Row 3 — The headline callout:**

Add a **Card** visual for `[High-Risk Attrition Rate]` (55.1%) with a text box next
to it reading: *"Employees working overtime with under 1 year of tenure leave at
55% — more than 3x the company average."* This is your single most important
sentence on the whole dashboard; make it visually dominant (large font, red accent).

---

## Page 2: Attrition Drivers

**Row 1 — Two KPI comparison cards:**
- `[Overtime Attrition Rate]` (30.5%) next to a second card showing the non-overtime
  rate for contrast — easiest way is one **Clustered Column Chart**: Axis =
  `OverTime`, Values = `[Attrition Rate]`. Two bars, dramatic difference, no need
  for two separate cards.

**Row 2 — Job Role breakdown** (Clustered Bar Chart, full width)
- Axis: `JobRole`
- Values: `[Attrition Rate]`
- Sort descending — Sales Representative (39.8%) will be the clear outlier at top.
  Add a reference line at 16.12% (the company average, `[Attrition Rate]` with no
  filter) so the outliers are visually obvious against the baseline.

**Row 3 — Two visuals side by side:**

*Left: Attrition Rate by Tenure Group* (Line and Clustered Column Chart or simple
Column Chart)
- Axis: `TenureGroup` — **manually sort** the axis in this order: `0-1 yr, 1-3 yrs,
  3-5 yrs, 5-10 yrs, 10+ yrs` (Power BI will alphabetize by default, which is wrong
  here — right-click the axis field → Sort by Column, or reorder via a sort-index
  column if needed)
- Values: `[Attrition Rate]`
- Shows the clear pattern: 34.9% in year 0-1, dropping steadily to 8.1% at 10+ years

*Right: Work-Life Balance vs Attrition* (Column Chart)
- Axis: `WorkLifeBalanceLabel`
- Values: `[Attrition Rate]`
- "Bad" work-life balance shows 31.2% attrition — more than double "Better" (14.2%)

**Row 4 — Business Travel** (small Column Chart, half width, paired with a text
insight box)
- Axis: `BusinessTravel`, Values: `[Attrition Rate]`
- Frequent travelers: 24.9% vs Non-Travel: 8.0%

---

## Page 3: Compensation & Demographics

**Row 1 — Income KPI cards:**
| Card | Value |
|---|---|
| `[Avg Monthly Income]` | $6,503 |
| `[Income Gap (Stayed vs Left)]` | $2,046 |

**Row 2 — Income Band vs Attrition** (Column Chart)
- Axis: `IncomeBand` (already ordered Low→Very High from the qcut in cleaning)
- Values: `[Attrition Rate]`
- The "Low" income quartile should show the highest attrition — direct visual
  support for the income-gap finding above.

**Row 3 — Two visuals side by side:**

*Left: Attrition by Age Group* (Column Chart)
- Axis: `AgeGroup`, Values: `[Attrition Rate]`
- 18-25 group shows 35.8% — by far the highest, dropping through career stages

*Right: Stock Option Level vs Attrition* (Column Chart)
- Axis: `StockOptionLevel`, Values: `[Attrition Rate]`
- Level 0 (no stock options): 24.4% attrition vs Level 1-2: under 10%. A concrete,
  actionable lever — this is a policy variable the company actually controls,
  unlike age or tenure.

**Row 4 — Table for detail-on-demand**
- A simple **Table** visual: `JobRole`, `Department`, `AttritionFlag` (as Avg, i.e.
  the rate), `[Avg Monthly Income]`, `[Total Headcount]`
- This lets a viewer who wants to check a specific role/department combination find
  it directly, satisfying "drill-down capability" without needing a separate page.

---

## Step 7: Slicers and cross-filtering (the "interactive" requirement)

1. Add a **Slicer** visual for `Department` — place top-left of Page 1
2. Add a **Slicer** visual for `JobRole`
3. Add a **Slicer** visual for `AgeGroup`
4. With slicers selected, go to **Format → Edit Interactions**, or simpler: use
   **View → Sync Slicers** pane to make these three slicers apply across all 3
   pages, not just the page they're placed on
5. Test: click "Sales" in the Department slicer, confirm every visual on every page
   updates — KPI cards, all charts, the table

This satisfies the task list's explicit requirement for "filters, slicers, or
drill-down capability" connected across the report, not isolated to one page.

---

## Step 8: Formatting pass (executive polish)

- **Theme:** View → Themes → pick a clean corporate theme (or set a custom navy/grey
  palette to match your Beginner-level workbook for visual consistency across your
  three submissions)
- **Titles:** Every visual needs a clear title (Format pane → General → Title) —
  don't leave Power BI's auto-generated field names as titles
- **Page names:** Rename the three page tabs at the bottom: "Overview", "Attrition
  Drivers", "Compensation & Demographics" — not "Page 1/2/3"
- **Tooltips:** Leave default tooltips on; they add drill-down value for free
- **Remove gridlines and unnecessary axis clutter** on chart visuals (Format pane →
  Gridlines → off) for a cleaner look, matching what you did in the Beginner Excel
  dashboard

---

## Step 9: Save and export

1. Save as `HR_Attrition_Dashboard.pbix`
2. File → Export → **Export to PDF** as a static backup (useful for the GitHub repo,
   since not everyone can open a live .pbix)
3. Take one screenshot of each of the 3 pages for your README

---

## What to say in your video about this level

You should be able to explain, in your own words:

1. **Why these KPIs** — Attrition Rate is the north star; every other measure exists
   to explain *why* it's 16.12% and where it's worse than average.
2. **Why no time-trend visuals** — this dataset is a snapshot, not a time series; a
   fake trend line would misrepresent the data, so the dashboard is built around
   categorical drill-downs instead. This is a deliberate design choice, not
   something missing.
3. **The headline finding** — overtime + low tenure compounds into 55% attrition,
   more than 3x the company average. This is the single most useful insight on the
   whole dashboard because it identifies a *specific, findable* at-risk group
   (rather than a vague "attrition is high" statement).
4. **A concrete, controllable lever** — Stock Option Level 0 has 2.4-3x the
   attrition of Levels 1-2. Unlike age or tenure, this is a policy the company can
   actually change.
