# IBM HR Attrition — Insights & Recommendations
### ShadowFox Data Analyst Internship — Advanced Level

**Dataset:** IBM HR Analytics Employee Attrition & Performance (Kaggle, fictional data
created by IBM data scientists) — 1,470 employees, 35 attributes, verified against
the standard published benchmark (16.12% attrition, 237 of 1,470).

**A note on the dataset itself:** this is a well-known, fictional teaching dataset,
not real company data. Its rates are not industry benchmarks and its patterns are
illustrative rather than something to generalize to a real workforce. The value here
is in demonstrating the *method* — how to go from raw HR data to KPI design to a
decision-oriented dashboard — which is exactly what this level is meant to evaluate.

---

## Why these KPIs were chosen

- **Attrition Rate** is the north-star metric — it's what an HR executive actually
  manages toward. Every other KPI and chart on the dashboard exists to explain *why*
  it sits at 16.12% and where it's worse than that average.
- **Overtime Attrition Rate** and **Income Gap** were chosen after an initial scan of
  every categorical field's relationship to attrition — overtime status produced by
  far the largest single split (30.5% vs 10.4%), so it earned a dedicated KPI rather
  than being buried in a generic bar chart.
- **High-Risk Attrition Rate** (overtime + tenure under 1 year) was added after
  noticing that overtime and low tenure independently mattered — testing whether they
  *compound* revealed 55.1%, which is a far more actionable finding than either
  factor alone: it names a specific, findable group of 69 employees rather than a
  vague risk category.
- **No time-based KPIs** (month-over-month trend, YoY change) were included, because
  this dataset is a single snapshot with no hire/exit date field — only a static
  `YearsAtCompany` value. Building a fake trend would misrepresent the data. This is
  a deliberate design decision the dashboard is built around, not a missing feature.

---

## Findings

1. **Overtime is the single strongest attrition driver.** Employees working overtime
   leave at **30.5%** vs **10.4%** for those who don't — a 3x difference, the largest
   gap of any binary split in the dataset.

2. **New hires under 1 year of tenure leave at 34.9%**, more than double the company
   average, and the rate declines steadily with tenure down to 8.1% at 10+ years.
   Retention risk is heavily front-loaded in the first year.

3. **These two factors compound sharply.** Employees who are both new (under 1 year)
   *and* working overtime leave at **55.1%** — over 3x the company average — while
   tenured employees not working overtime leave at just **8.0%**. This is the
   dashboard's headline finding because it identifies a specific, addressable
   69-person group rather than a diffuse trend.

4. **Sales Representative is the highest-risk single role** at 39.8% attrition —
   more than double the next-highest role (Laboratory Technician, 23.9%) — despite
   Sales Representatives not being an unusually large group (83 employees).

5. **Compensation matters, but less than overtime or tenure.** Employees who leave
   earn $2,046/month less on average than those who stay ($4,787 vs $6,833), and the
   lowest income quartile shows 29.3% attrition vs ~10-14% for the top three
   quartiles. Pay is a real factor, but the overtime and tenure effects are larger.

6. **Stock Option Level 0 shows the highest attrition (24.4%)** versus Levels 1-2
   (under 10%) — and unlike age or tenure, **this is a lever the company can
   actually pull**, since stock option allocation is a policy decision, not an
   employee characteristic.

7. **Work-life balance and travel frequency both show clear gradients.** "Bad" work-
   life balance rating: 31.2% attrition vs 14.2% for "Better." Frequent travelers:
   24.9% vs 8.0% for non-travelers.

8. **Age shows the steepest single gradient in the dataset:** 35.8% attrition in the
   18-25 group, falling to 9.2% at 36-45, before ticking back up slightly at 56-65
   (likely retirement-related rather than dissatisfaction-related — the data can't
   distinguish these, which is a genuine limitation).

---

## Recommendations

| # | Recommendation | Evidence | Who owns it |
|---|---|---|---|
| 1 | **Build a structured first-year onboarding and check-in program**, with extra attention to any new hire also working overtime | 55.1% attrition in this exact overlap group vs 8.0% baseline | HR / People Ops |
| 2 | **Audit overtime allocation, especially for new hires** — cap or actively monitor overtime hours in an employee's first 12 months | Overtime alone triples attrition; combined with low tenure it's the single largest risk multiplier found | Department managers |
| 3 | **Review Sales Representative role design specifically** — comp structure, workload, or manager support | 39.8% attrition, nearly 2.5x the company average, isolated to one role | Sales leadership |
| 4 | **Expand stock option eligibility at Level 0**, or introduce an earlier vesting milestone | 24.4% attrition at Level 0 vs under 10% at Levels 1-2; this is a controllable policy lever, not a fixed employee trait | Compensation & Benefits |
| 5 | **Review compensation specifically for the lowest income quartile**, not company-wide | 29.3% attrition in the lowest quartile vs ~10-14% elsewhere; the gap between quartiles 2-4 is small, so this isn't a broad pay problem | Compensation & Benefits |
| 6 | **Investigate travel policy for frequent travelers** — trip caps, remote-alternative options, or added support | 24.9% attrition for frequent travelers vs 8.0% for non-travelers | Department managers |

**Sequencing:** Recommendations 1 and 2 target the highest-leverage finding (the
55.1% compounding group) and should be first, since the affected population (69
employees) is small enough to act on quickly. Recommendation 4 is the next fastest
to implement, since it's a benefits-policy change rather than a role redesign.
Recommendation 3 (Sales Rep role review) is the most structurally significant but
will take longest to show results.

---

## Method notes and limitations

- Ordinal survey fields (Education, JobSatisfaction, EnvironmentSatisfaction,
  WorkLifeBalance, JobInvolvement, PerformanceRating, JobLevel) were decoded from
  raw 1–4/1–5 integers into readable labels before analysis — an unlabelled "3" on a
  satisfaction scale is not interpretable to a dashboard viewer without this step.
- `AgeGroup`, `TenureGroup`, and `IncomeBand` are derived bins created specifically
  to support slicer-based drill-down in Power BI, not part of the raw IBM data.
- The age-attrition uptick at 56-65 is very likely retirement rather than
  dissatisfaction, but the dataset has no exit-reason field to confirm this — stated
  as a limitation rather than assumed.
- This is a fictional, single-snapshot dataset. Findings demonstrate analytical
  method (KPI design, driver analysis, compounding-factor detection) and should not
  be read as real-world HR benchmarks or transferable conclusions about any actual
  organization.
- No time-series analysis is possible or attempted, for the reason stated above.

---

## Files in this deliverable

```
HR_Attrition_PowerBI_Ready.csv     Cleaned, labeled dataset — import this into Power BI
DAX_Measures.md                    Every DAX formula needed, with expected values
Dashboard_Build_Guide.md           Page-by-page build instructions
Dashboard_Mockup_Reference.html    Visual reference of the finished 3-page dashboard
Insights_and_Recommendations.md    This file
```
