# Employee Attrition Analysis Power BI Dashboard

An interactive 3-page Power BI dashboard analyzing employee attrition patterns using IBM's publicly released HR Analytics dataset — built to answer a real business question: **why are employees leaving, and who is most at risk?**

---

## Project Goal

Employee turnover is expensive and disruptive for any organization. This project analyzes 1,470 employee records to uncover **where, why, and who** attrition is concentrated — turning a static HR dataset into a decision-support tool a manager could actually use.

Rather than presenting isolated charts, the dashboard is structured as three connected questions:

1. **Overview** — What does attrition look like across the company?
2. **Why They Leave** — What working conditions correlate with people leaving?
3. **Who Leaves** — Which employee groups are most at risk?

---

## Dashboard Pages

### Page 1 — Overview
![Overview](overview.png)

Sets the baseline with three headline KPIs (overall attrition rate, headcount, average monthly income), then breaks attrition down by department, overtime status, job role compensation, and tenure — giving a first read on *where* the problem is concentrated before digging into *why*.

### Page 2 — Why They Leave
![Why They Leave](why_they_leave.png)

Focuses on working-condition factors: job satisfaction, work-life balance, and commute distance. This page is aimed at the "what can we actually change?" question — factors a company has some control over, as opposed to fixed demographics.

### Page 3 — Who Leaves
![Who Leaves](who_leaves.png)

Breaks attrition down by demographic and personal factors: gender, marital status, age group, and education level — identifying which employee segments carry the highest risk.

All three pages share a `Department` slicer, so any finding can be immediately filtered down to a specific department for a more targeted view.

---

## Key Findings

- **Overall attrition rate: 16.12%** — roughly 1 in 6 employees left, in line with the dataset's known baseline.
- **Sales has the highest attrition rate**, followed by HR, with Research & Development the most stable department — suggesting turnover is not evenly distributed and may be tied to role pressure or compensation structure rather than company-wide culture.
- **Employees working overtime leave at a substantially higher rate** than those who don't — one of the clearest and most actionable signals in the dataset.
- **Attrition is highest in the first 0-2 years of tenure** and drops sharply afterward — the first two years appear to be the highest-risk retention window, not late-career transitions.
- **Low job satisfaction and poor work-life balance both correlate with higher attrition** — reinforcing that this is a controllable, working-conditions problem rather than a purely demographic one.

---

## Tech Stack

- **Power BI Desktop** — data modeling, DAX measures, interactive visuals
- **DAX** — custom measures (Attrition Rate) and conditional grouping logic (tenure bands, age bands)
- **Power Query (M)** — data transformation: custom columns, categorical binning, sort-order handling

---

## Key DAX Measure

```dax
Attrition Rate = 
DIVIDE(
    CALCULATE(COUNTROWS('HR Data'), 'HR Data'[Attrition] = "Yes"),
    COUNTROWS('HR Data')
) * 100
```

This single measure powers every chart in the dashboard — recalculating dynamically as slicers filter the underlying data, so "Attrition Rate by Department" and "Attrition Rate filtered to Female employees in Sales" use the exact same logic with zero duplicated code.

---

## Modeling Notes

Two numeric fields (`YearsAtCompany`, `Age`) were originally plotted as continuous lines, which produced a misleading, noisy trend — a handful of employees with rare, very high tenure values swung the rate wildly (a classic small-sample-size distortion). Both were re-grouped into meaningful bands (e.g. `0-2 years`, `3-5 years`, `6-10 years`...) using a Power Query custom column, with a paired numeric "sort" column to keep the bands in logical rather than alphabetical order. This produced a much clearer, more trustworthy trend and is a good example of a common pitfall in exploratory analysis: **a technically correct chart can still be visually misleading if sample size per category isn't considered.**

---

## Dataset

[IBM HR Analytics Employee Attrition & Performance](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset) — a fictional dataset created by IBM data scientists, 1,470 records × 35 attributes, publicly available on Kaggle.

---

## About

Built as part of my transition into Data Analytics, with a focus on turning a raw dataset into a decision-ready tool: defining the right business questions, choosing measures deliberately, and catching (and fixing) a real analytical pitfall along the way rather than presenting a first-pass result at face value.
