# 📊 Data Job Market Analysis: Salaries & Skills with Advanced DAX

> Do more skills equal more money for data professionals? An Excel Power Pivot project that uses a Data Model and custom DAX measures to explore salary, location, and skill demand across ten data and tech job titles.

View work: [DAX Advanced.xlsx](https://github.com/user-attachments/files/34578078/DAX.Advanced.xlsx)

---

## 🎯 Project Overview

The data job market is crowded with advice about "the skills you need." This project tests that advice against the data. Using a two-table **Excel Data Model** (job postings and the skills attached to them), I built reusable **DAX measures** and interactive **PivotTables, PivotCharts, and slicers** to answer:

1. Which data roles pay the most, and how does the **US compare with the rest of the world**?
2. Do roles that demand **more skills** actually pay more?
3. Which skills are **most in demand**, and which are **best paid**?

## 🔑 Key Findings

| # | Insight | Evidence |
|---|---------|----------|
| 1 | **Seniority beats skill count.** More skills per posting does not mean higher pay. | Senior Data Scientists earn the most (**$155,000**) with ~5.3 skills per job, while Senior Data Engineers list the most skills (~8.1) but earn less (**$147,500**). |
| 2 | **Business Analyst is both the lowest-paid and the least skill-heavy role.** | **$85,000** median with ~3.3 skills per job, vs. a **$115,000** overall median across all roles. |
| 3 | **The US pays a premium, but unevenly.** | Overall median is **$118,940 (US)** vs **$111,175 (non-US)**. The gap is largest for Machine Learning Engineers (**$150,000 vs $101,029**) and Software Engineers (**$125,000 vs $89,100**). Data Analyst pay is identical at **$90,000**. |
| 4 | **SQL and Python dominate demand.** | SQL appears in **~57%** of postings and Python in **~54%**, far ahead of Tableau (~22%), R (~21%), and AWS (~21%). |
| 5 | **Most in-demand does not mean best paid.** | **Spark ($140,000)**, **AWS ($135,000)**, and **Java (~$134,000)** command the highest medians, while Excel is lowest (**$92,500**). |
| 6 | **Big-data and cloud skills carry a pay premium.** | Spark and AWS sit well above the **$117,500** all-skills median despite appearing in far fewer postings than SQL or Python. |

> **Takeaway:** SQL and Python are the entry ticket. Pay is driven more by seniority and by specialising in scalable and cloud tooling (Spark, AWS) than by accumulating a long list of skills.

## 🗂️ Workbook Structure

| Sheet | Purpose | Key measures |
|-------|---------|--------------|
| `Salary_vs_Skills` | Scatter chart: *"Do more skills equal more money for Data Nerds?"* | `Median Salary`, `Skills Per Job` |
| `Salary_Analysis` | Median salary by job title, split US vs non-US | `Median Salary`, `Median Salary US`, `Median Salary Non-US` |
| `Skills_job_analysis` | Bar chart: *"What are the Top Skills of Data Nerds?"* | `Skill Likelihood` |
| `Skill_Salary_Analysis` | Combo chart: *"What's the pay of Top 10 skills?"* | `Median Salary - Skills`, `Skill Likelihood` |

**Interactivity:** every sheet has **slicers** (Job Title and/or Country) so the analysis can be filtered live.

## 🧠 Data Model & DAX

Two tables are loaded through **Power Query** into the Excel **Data Model** and related on `job_ID`:

- `data_jobs_salary`: one row per job posting (title, country, location, average yearly salary, etc.)
- `data_jobs_skills`: one row per job and skill pair (`job_ID`, `job_skills`)

### Core measures

```dax
-- Typical pay (median is robust to salary outliers)
Median Salary = MEDIAN ( data_jobs_salary[salary_year_avg] )

-- Geographic comparison
Median Salary US =
CALCULATE ( [Median Salary], data_jobs_salary[job_country] = "United States" )

Median Salary Non-US =
CALCULATE ( [Median Salary], data_jobs_salary[job_country] <> "United States" )

-- Average number of skills listed per posting
Skills Per Job =
DIVIDE (
    COUNT ( data_jobs_skills[job_skills] ),
    DISTINCTCOUNT ( data_jobs_skills[job_ID] )
)

-- Share of all postings that mention the selected skill(s)
Skill Likelihood =
DIVIDE (
    DISTINCTCOUNT ( data_jobs_skills[job_ID] ),
    CALCULATE ( DISTINCTCOUNT ( data_jobs_skills[job_ID] ), ALL ( data_jobs_skills[job_skills] ) )
)

-- Median salary of postings that require a given skill
Median Salary - Skills = MEDIAN ( data_jobs_salary[salary_year_avg] )
```

**Why DAX instead of plain PivotTable math?**
- `MEDIAN` is not available as a standard PivotTable summary, and it handles skewed salary data better than the average.
- `CALCULATE` lets one pivot show **US and non-US medians side by side** without duplicating the data.
- Measures react to slicers and filter context, so one definition powers every sheet.

## 🛠️ Tools & Skills Demonstrated

- **Microsoft Excel** (Power Pivot / Data Model, PivotTables, PivotCharts, Slicers)
- **DAX**: `CALCULATE`, `MEDIAN`, `DIVIDE`, `DISTINCTCOUNT`, `ALL`, filter context
- **Power Query**: data import and shaping
- **Data modelling**: relating fact and dimension-style tables on a shared key
- **Data storytelling**: turning pivot output into business-ready insights


⭐ If you found this useful, consider starring the repo!
