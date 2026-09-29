# ShadowFox Data Analyst Internship — [Your Name]

Three progressive levels of analysis on the **Global Superstore** dataset (Beginner,
Intermediate) and the **IBM HR Analytics** dataset (Advanced), built with Excel, Python,
and Power BI respectively.

| Level | Tooling | Folder |
|---|---|---|
| Beginner | Excel (formulas, PivotChart-style analysis, dashboard) | [`01_beginner_excel_dashboard/`](01_beginner_excel_dashboard) |
| Intermediate | Python (pandas, RFM/cohort analysis, matplotlib) | [`02_intermediate_python_analysis/`](02_intermediate_python_analysis) |
| Advanced | Power BI (DAX measures, interactive dashboard) | [`03_advanced_powerbi_dashboard/`](03_advanced_powerbi_dashboard) |

Raw and cleaned source data for the Beginner/Intermediate levels lives in [`data/`](data)
so both levels share one verified, correctly-parsed dataset.

---

## data/

- `Global_Superstore2.csv` — original public dataset (51,290 rows, 2011–2014).
- `cleaned_superstore.csv` — cleaned version used by both the Beginner workbook and the
  Intermediate notebook (parsed with `thousands=','`, decoded categorical fields, derived
  columns for date/tenure grouping). See the Intermediate report §2 for why a naive
  `pd.to_numeric(..., errors='coerce')` silently drops every value ≥ $1,000 on this file,
  and how that was caught and fixed.

## 01_beginner_excel_dashboard/

- `Global_Superstore_Beginner_Dashboard.xlsx` — the deliverable. 8 sheets: Read Me,
  Dashboard Overview, KPI Summary, Category & Segment, Regional Analysis, Monthly Trend,
  Sub-Category Performance, Insights & Conclusions, Raw Data. Every number is a live
  SUMIFS/AVERAGEIFS/COUNTIFS formula over an Excel Table, not a pasted value.
- `build_workbook.py` — the script that generates the workbook from
  `../data/cleaned_superstore.csv`, for reproducibility.

## 02_intermediate_python_analysis/

- `Intermediate_Analysis.ipynb` — runs top to bottom from `../data/Global_Superstore2.csv`.
  Covers data-quality validation, RFM segmentation, cohort retention, revenue-growth
  decomposition (existing vs. new customers), and discount/profitability analysis.
- `Intermediate_Analysis_executed.ipynb` — the same notebook with outputs already run, so
  it can be reviewed without re-executing.
- `Intermediate_Report.md` — the written report: findings, a growth-decomposition
  waterfall chart, and 7 evidence-backed recommendations (including an A/B-test proposal
  to validate the discount-cap recommendation before a full rollout).
- `charts/` — the 11 figures referenced in the report.

## 03_advanced_powerbi_dashboard/

- `HR_Attrition_PowerBI_Ready.csv` — cleaned IBM HR Analytics dataset, ready to import.
- `DAX_Measures.md` — every measure used in the dashboard, with its expected value.
- `Dashboard_Build_Guide.md` — page-by-page build instructions.
- `Dashboard_Mockup_Reference.html` — a static visual reference of the finished dashboard
  (open in any browser).
- `screenshots/` — screenshots of the actual built `.pbix` (3 pages).
- `HR_Attrition_Dashboard.pbix` — **add this file yourself**: Power BI Desktop files were
  built and verified interactively but can't be produced or exported from this repo's
  build environment. Build it by following `Dashboard_Build_Guide.md`, then export a PDF
  backup and save the `.pbix` here before final submission.

---

## Reproducing everything

```
data/cleaned_superstore.csv          already generated; source: data/Global_Superstore2.csv
01_beginner_excel_dashboard/          python3 build_workbook.py   (needs openpyxl)
02_intermediate_python_analysis/      jupyter nbconvert --to notebook --execute Intermediate_Analysis.ipynb
03_advanced_powerbi_dashboard/        open HR_Attrition_PowerBI_Ready.csv in Power BI Desktop,
                                       follow Dashboard_Build_Guide.md
```

## Submission

Per the task list: mentor contact / task validation → g2doubts@shadowfox.in. Final
submission = this repository + a 3–5 minute self-recorded video (Google Drive link) in
the submission form.
