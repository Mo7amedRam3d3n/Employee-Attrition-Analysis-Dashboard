# HR Attrition & Workforce Analytics Dashboard

An interactive **Power BI** dashboard analyzing employee attrition data for **1,470 employees**, built to uncover *why* employees leave and *which employee segments* are most at risk — not just how many left overall.

---

## 📌 Overview

The company has an overall attrition rate of **16%** (237 out of 1,470 employees left). On the surface this looks moderate — but breaking it down by category reveals specific groups with attrition rates 2–3x higher than the company average.

This dashboard was built as a single, information-dense page combining KPI cards, tables, and interactive charts to answer:
- Who is leaving, and from which departments/roles?
- What personal and job-related factors correlate with attrition?
- Where should retention efforts be focused first?

---

## 🗂️ Dataset

The dataset resembles the well-known IBM HR Attrition dataset structure, with the following fields:

| Column | Description |
|---|---|
| `emp_no` | Unique employee ID |
| `gender`, `marital_status`, `age`, `age_band` | Demographics |
| `education`, `education_field` | Education background |
| `department`, `job_role` | Job information |
| `business_travel` | Travel frequency (Non-Travel / Travel_Rarely / Travel_Frequently) |
| `job_satisfaction` | Satisfaction score (1–4) |
| `attrition` / `attrition_label` | Whether the employee left (Yes/No — Current/Ex-Employees) |
| `active_employee` | 1 = currently active, 0 = left |
| `employee_count` | Constant helper column (always 1), used for aggregation |

---

## 🧹 Data Preparation (Power Query)

- Verified and corrected data types (numeric vs. text columns)
- Checked for duplicate `emp_no` values
- Checked for null/blank values across all columns
- Trimmed and cleaned text columns (`department`, `job_role`, `education_field`) to remove inconsistent spacing/casing
- Validated logical consistency between `attrition`, `attrition_label`, and `active_employee`
- Cross-checked `age_band` against `age` for correct bucketing

---

## 📐 Key DAX Measures

\`\`\`DAX
Total Employees = COUNTROWS('HR Data')

Attrition Count = 
CALCULATE(
    COUNTROWS('HR Data'),
    'HR Data'[attrition] = "Yes"
)

Attrition Rate % = 
DIVIDE([Attrition Count], [Total Employees], 0)

Active Employees = [Total Employees] - [Attrition Count]

Avg Job Satisfaction = AVERAGE('HR Data'[job_satisfaction])
\`\`\`

> **Note:** `Attrition Rate %` is context-sensitive — when placed alongside a category (e.g., `department`), it automatically calculates the rate *within that category* (leavers ÷ total employees of that category), not against the company total. This distinction is central to the dashboard's insights (see below).

---

## 📊 Dashboard Layout

A single-page, dark-themed dashboard with:

**Top row — KPI cards:** Total Employees, Attrition Count, Attrition Rate %, Active Employees, Avg Age, Avg Satisfaction

**Main body — 5 visuals:**
- **Attrition by Business Travel** (table) — attrition count and rate % for each travel frequency group (Non-Travel / Travel_Rarely / Travel_Frequently)
- **Attrition Rate % by Department & Marital Status** (cross-tab table) — attrition rate % broken down by department (rows) and marital status (columns)
- **Attrition by Department** (donut chart) — each department's share of total company-wide attrition
- **Attrition Count and Rate % by Job Role** (bar chart) — attrition count per job role, with rate % shown on hover
- **Employee Distribution: Current vs Ex-Employees** (donut chart) — overall split between active and former employees

**Bottom — Age range slicer** (18–60), filtering every visual on the page

---

## 💡 Key Insights

1. **Travel frequency is a top attrition driver** — employees who travel frequently leave at **25%**, vs. only **8%** for those who don't travel at all (3x difference).
2. **Marital status matters** — single employees leave at **26%**, compared to **12%** for married employees.
3. **Raw counts can mislead** — R&D has the highest *number* of leavers (133, or 56% of all attrition), simply because it's the largest department. But when measured as a *rate*, **Sales has the highest attrition rate (21%)** vs. R&D's 14% — meaning Sales is proportionally riskier per employee, even though R&D contributes a larger absolute share of total attrition.
4. **Job role risk** — Laboratory Technicians have both a high count (62) and a high rate (24%), making this role a genuine risk area, not just a volume effect.

**Bottom line:** The employee profile most at risk of leaving is a **single employee who travels frequently and works in Sales or as a Laboratory Technician**. Absolute counts show where the *volume* of the problem is; rates show where the *real risk* is — both are needed to avoid drawing the wrong conclusion.

---

## 🛠️ Tools Used

- **Power BI Desktop** (Power Query,Power Bi ,DAX, visuals)
