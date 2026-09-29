# DAX Measures — IBM HR Attrition Dashboard

Paste each of these into Power BI as a **New Measure** on the `HR_Attrition` table
(Home → New Measure, or right-click the table in Fields pane).
Every number below has been verified in Python against the actual data first —
these are not guesses, they're the numbers you should see appear in Power BI.

---

## Core KPIs (put these on every page as KPI cards)

```dax
Total Headcount = COUNTROWS(HR_Attrition)
```
Expected value: **1,470**

```dax
Attrition Count = CALCULATE(COUNTROWS(HR_Attrition), HR_Attrition[Attrition] = "Yes")
```
Expected value: **237**

```dax
Attrition Rate = DIVIDE([Attrition Count], [Total Headcount], 0)
```
Expected value: **16.12%** (format as Percentage, 2 decimals)

```dax
Active Headcount = [Total Headcount] - [Attrition Count]
```
Expected value: **1,233**

```dax
Avg Monthly Income = AVERAGE(HR_Attrition[MonthlyIncome])
```
Expected value: **$6,503**

```dax
Avg Tenure (Years) = AVERAGE(HR_Attrition[YearsAtCompany])
```
Expected value: **7.0**

```dax
Avg Age = AVERAGE(HR_Attrition[Age])
```
Expected value: **36.9**

---

## Comparison / driver measures

```dax
Overtime Attrition Rate =
CALCULATE([Attrition Rate], HR_Attrition[OverTime] = "Yes")
```
Expected value: **30.5%** (vs 10.4% for non-overtime — this is your single biggest driver)

```dax
Income Gap (Stayed vs Left) =
CALCULATE([Avg Monthly Income], HR_Attrition[Attrition] = "No")
  - CALCULATE([Avg Monthly Income], HR_Attrition[Attrition] = "Yes")
```
Expected value: **$2,046** (people who leave earn ~$2K/month less on average)

```dax
High-Risk Headcount =
CALCULATE(
    COUNTROWS(HR_Attrition),
    HR_Attrition[OverTime] = "Yes",
    HR_Attrition[TenureGroup] = "0-1 yr"
)
```
Expected value: **69**

```dax
High-Risk Attrition Rate =
CALCULATE(
    [Attrition Rate],
    HR_Attrition[OverTime] = "Yes",
    HR_Attrition[TenureGroup] = "0-1 yr"
)
```
Expected value: **55.1%** — this is your headline callout stat. New hires working
overtime leave at more than 3x the company average.

```dax
Low-Risk Attrition Rate =
CALCULATE(
    [Attrition Rate],
    HR_Attrition[OverTime] = "No",
    HR_Attrition[TenureGroup] <> "0-1 yr"
)
```
Expected value: **8.0%**

---

## Formatting notes

- All `Rate` measures: format as **Percentage**, 1 decimal place.
- All `Income` measures: format as **Currency ($)**, 0 decimals.
- `Total Headcount`, `Attrition Count`, `Active Headcount`, `High-Risk Headcount`: **Whole Number**.

## A note on what this dashboard can't show

This dataset is a **single snapshot**, not a time series — there's no hire date or
exit date field, only `YearsAtCompany` (a static tenure value at the time of
snapshot). That means **no month-over-month or trend-over-time visuals are possible
here**, and you shouldn't try to fake one. The dashboard is designed instead around
categorical drill-downs (department, role, tenure band, overtime status) rather than
a timeline — that's a deliberate choice given the data, not a gap. Say this
explicitly if asked in your video; it shows you understood the data's limits rather
than missing them.
