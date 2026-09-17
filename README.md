# HR Analytics Dashboard — Power BI

<img width="838" height="299" alt="Image" src="https://github.com/user-attachments/assets/e5e9cad4-6d0d-436c-a6ee-39b5ef5f7fa4" />

A five-page Power BI report analysing workforce data for 1,000 employees across three countries, built to answer the questions an HR manager actually acts on: who is leaving, when, where pay sits, and where experience is about to walk out the door.

---

## The headline finding

**47% of all employee exits happen within the first three years** — 20 of 85 within the first year alone. Employees who pass that point tend to stay a decade or more.

This reframes the retention problem entirely: the issue is onboarding, not long-service loyalty.

---

## Report structure

| Page | Question it answers |
|---|---|
| **Executive Summary** | What should I know before reading anything else? |
| **Workforce Overview** | What happened this year, and where? |
| **Workforce Demographics** | Who works here? |
| **Compensation & Pay Equity** | Is pay fair? |
| **Retirement Risk & Geography** | Where are we about to lose experience? |

Each page opens with a stated claim and the visuals below it serve as evidence, rather than presenting metrics without interpretation.

<img width="826" height="329" alt="Image" src="https://github.com/user-attachments/assets/d3bd8b51-3166-49c8-97ad-0c77a881854c" />

 
<img width="838" height="299" alt="Image" src="https://github.com/user-attachments/assets/bac1fc23-92ab-413c-902f-2fc4048d99ce" />


 
<img width="743" height="296" alt="Image" src="https://github.com/user-attachments/assets/00aab0cd-ac4a-489a-920f-221dcb0ac512" />


 
<img width="746" height="300" alt="Image" src="https://github.com/user-attachments/assets/819ad891-1b30-418f-b8ce-89a995dbd634" />


 
<img width="733" height="298" alt="Image" src="https://github.com/user-attachments/assets/6dfcb9fc-51d6-437f-9dea-51cb04ecd866" />



---

## Key findings

**Turnover reached 2.2% in 2021** — its highest since 2004, and the peak of a rise that began in 2017. This happened during a year of expansion (86 new hires, +30%).

**Engineering is squeezed from both ends** — the highest turnover of any department (5.0%) and, in China, the highest retirement exposure in the company (42.3%). One function losing new hires early and veterans soon.

**Retirement risk has not improved — it was masked by hiring.** 217 employees are aged 55+, and that count has not moved since 2019. The percentage fell only because the headcount denominator grew.

**The gender pay gap reflects role distribution, not unequal pay.** The median gap is −0.5%, and compa-ratio sits between 0.98 and 1.02 within every job title. The gaps visible at department level come from who holds which roles, not from different pay for the same work.

**Bonuses apply at Manager level and above only** — zero for 25+ individual-contributor roles, applied consistently across all three countries.

---

## Technical implementation

**Data model** — star schema with `Fact_Employees` and four dimensions (`Dim_Date`, `Dim_Job`, `Dim_Location`, `Dim_Employee`).

**DAX** — 60+ measures covering headcount, turnover, tenure, compensation and pay equity. Notable patterns:

- Point-in-time headcount using `REMOVEFILTERS('Dim_Date')` with hire/exit date boundaries
- Compa-ratio at two grains: row-level (`ALLEXCEPT` on job title) for tables, and an `AVERAGEX` variant that is safe to use in a single KPI card
- Median-based pay gap alongside the mean-based version, since the two answer different questions
- Calculated columns for tenure-at-exit banding, with a numeric sort column

**KPI cards** — built as HTML/CSS strings in DAX and rendered through the HTML Content visual, giving control over layout, conditional colour and animation that native cards do not offer.

---

## Analytical decisions worth explaining

These are the judgement calls behind the report, documented because the reasoning matters more than the output.

**Median over mean for pay reporting.** Mean salary is $114,033; median is $96,693 — a $17K gap caused by a small number of very high earners. The mean-based gender pay gap reads +2.4%; the median-based gap reads −0.5%. The difference between the two is itself the finding: the gap lives at the top of the distribution, not in the middle.

**Small-sample suppression.** Brazil's Accounting department shows a −76% pay gap, but that figure rests on 8 people, only 2 of them men. A `Gender Pay Gap % (Reliable)` measure returns blank below 20 employees or 5 of either gender, and headcount is exposed in tooltips throughout.

**Tracking counts, not just rates.** The `% Nearing Retirement` card displays the absolute count beside the percentage, and its trend indicator follows the count. Without this, hiring makes a succession risk appear to improve when nothing has changed.

**Prior-year comparison guards.** All 18 PY measures return blank unless exactly one year is selected. Without the guard, selecting "All" compares a 30-year cumulative figure against a single year — a turnover card reading 18.6% against a prior year of 2.2%.

**Conditional alert indicators.** A pulsing status dot appears only on metrics with an unambiguously bad direction (rising turnover, rising compensation cost, rising retirement count). Metrics whose direction depends on context — headcount, new hires, average tenure, average age — show the change without implying a judgement the data cannot support.

---

## Data limitations

The dataset is a public practice file, and its characteristics were verified rather than assumed. These limits are stated inside the report itself:

- **2022 is incomplete.** Hiring is recorded to Dec 2021, exits to Aug 2022. The analysis year is 2021.
- **Salaries are not cost-of-living adjusted.** Pay ranges are near-identical across all three countries ($40K–$258K), driven by job title alone. Geographic pay comparisons are not meaningful here.
- **Ethnicity is confounded with country.** China is 100% Asian and Brazil 100% Latino, so ethnicity analysis is valid only within the United States.
- **Turnover is unrealistically low.** 85 exits across 30 years (0.3%–2.2% annually) against a real-world norm of 10–20%. Conclusions rest on direction, concentration and timing rather than absolute levels.
- **No exit reasons are recorded.** The report identifies where and when attrition occurs, not why. Findings indicate where to investigate.

---

## Tools

Power BI Desktop · DAX · Power Query · HTML/CSS (for custom KPI cards)

---

## Author

**Leena A. Elsheikh** — Data Analyst
MSc Statistics · MBA · Microsoft PL-300 (Power BI Data Analyst)
Riyadh, Saudi Arabia
