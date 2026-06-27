[README (2).md](https://github.com/user-attachments/files/29407959/README.2.md)
# Insurance-Investigation-MIS
End-to-end MIS reporting system for insurance investigation teams - Excel, Power BI, SQL, Python
# 🔍 Insurance Investigation MIS Tracker

**Tools Used:** Microsoft Excel · Power BI · SQL (MySQL) · Python (pandas)**

---

## 📌 Project Overview

Insurance companies outsource fraud investigation to third-party investigation firms.
These firms manage hundreds of cases daily — tracking **who is investigating what**,
**whether cases are closed on time (SLA)**, and **how much fraud is being detected**.

This project builds a complete **MIS reporting system** for such a team — from raw case
data all the way to a management-ready Power BI dashboard.

---

## 🎯 Business Problem

The investigation team had no centralised reporting system. Managers had to manually
count cases in Excel, had no visibility into SLA breaches until they happened, and
could not identify which investigators were underperforming. Reports to insurers were
delayed and inconsistent.

**This MIS tracker solves all of that.**

---

## 📊 Key Metrics Tracked

| Metric | Description |
|---|---|
| **TAT (Turnaround Time)** | Days from case assigned → case closed |
| **SLA Breach** | Whether TAT exceeded the agreed SLA limit (High: 3d / Med: 7d / Low: 14d) |
| **Fraud Identified** | Cases where investigation confirmed fraud |
| **Investigator Productivity** | Cases closed per investigator |
| **Case Closure Rate** | % of total cases closed |
| **Fraud Detection Rate** | Fraud cases ÷ closed cases × 100 |

---

## 🗂️ Project Structure

```
Insurance-Investigation-MIS/
│
├── Insurance_Investigation_MIS.xlsx   ← Full Excel workbook (dataset + MIS)
│   ├── Raw_Data                       ← 300 realistic investigation cases
│   ├── Daily_MIS_Report               ← KPI summary (COUNTIFS / SUMIFS)
│   ├── Monthly_Summary                ← Month-wise + insurer breakdown
│   ├── Pending_Cases_Tracker          ← Open cases with SLA risk flags
│   └── SQL_Reference                  ← 6 SQL queries on same dataset
│
├── Insurance_MIS_Dashboard.pbix       ← Power BI dashboard file
│
├── automate_report.py                 ← Python: auto-calculate TAT + flag SLA breaches
│
├── sql_queries.sql                    ← All SQL queries in one file
│
└── README.md                          ← This file
```

---

## 🧮 Excel — What Was Built

- **300 rows** of realistic insurance investigation case data
- **COUNTIFS / SUMIFS** formulas for all KPIs (no hardcoded values)
- **Monthly trend table** using SUMPRODUCT with MONTH() logic
- **Conditional formatting** to highlight SLA-breached rows in red
- **Auto risk flag** column: 🔴 SLA Breached / 🟡 At Risk / 🟢 OK
- **Pivot Tables** for investigator and insurer breakdowns

---

## 📈 Power BI Dashboard

> *(Screenshot: dashboard.png)*

**Visuals built:**
- Card KPIs: Total Cases | Open | SLA Breach % | Fraud Rate %
- Bar Chart: Cases closed per investigator (productivity)
- Donut Chart: Case status breakdown (Open / Closed)
- Line Chart: Monthly case volume trend
- Matrix Table: SLA breaches by region and priority
- Slicers: Filter by Insurer / Region / Month / Priority

**DAX Measures used:**
```dax
SLA Breach % = 
DIVIDE(
    COUNTROWS(FILTER('cases', 'cases'[SLA_Breach] = "Yes")),
    COUNTROWS(FILTER('cases', 'cases'[Status] = "Closed"))
) * 100

Fraud Detection Rate % = 
DIVIDE(
    COUNTROWS(FILTER('cases', 'cases'[Fraud_Identified] = "Yes")),
    COUNTROWS(FILTER('cases', 'cases'[Status] = "Closed"))
) * 100

Avg TAT (Closed) = 
AVERAGEX(FILTER('cases', 'cases'[Status] = "Closed"), 'cases'[TAT_Days])
```

---

## 💻 SQL Queries

**Database:** MySQL (same data as Excel, imported as `cases` table)

Queries cover:
1. Case volume by status
2. SLA breach rate by region
3. Investigator productivity + average TAT
4. Fraud cases by insurer
5. Monthly case volume trend
6. Top 10 high-priority SLA-breached cases

See full queries in `sql_queries.sql`

---

## 🐍 Python Automation

`automate_report.py` does the following:
1. Reads raw Excel data using pandas
2. Auto-calculates TAT from Assigned_Date and Closed_Date
3. Flags SLA breaches based on priority-specific SLA limits
4. Outputs a formatted weekly summary report as a new Excel sheet

```bash
python automate_report.py
# Output: Weekly_MIS_Report_<date>.xlsx
```

---

## 💡 Key Learnings

- Understood insurance investigation domain: TAT, SLA, fraud identification workflow
- Built dynamic MIS reports using COUNTIFS/SUMIFS that update automatically
- Created DAX measures in Power BI for operational KPIs
- Wrote SQL queries to aggregate and analyse 300+ case records
- Automated routine report generation using Python/pandas

---

## 👤 Author

**Siddharth Kamble**  
MSc Data Science (Distinction) — University of Essex, UK  
[LinkedIn](#) · [GitHub](#) · siddharthkamble2196@gmail.com
