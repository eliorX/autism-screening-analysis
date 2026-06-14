# Autism Spectrum Disorder Screening Data Analysis
### Tools: SQL · Microsoft Excel
### Dataset: ASD Screening Dataset — 704 records, 21 variables

---

## Project Overview

This project explores a real-world clinical screening dataset to identify which demographic and behavioural factors are most associated with a positive Autism Spectrum Disorder (ASD) screening outcome. Using SQL for data extraction and querying, and Microsoft Excel for dashboard visualisation, this analysis demonstrates how structured data can surface meaningful patterns in neurodevelopmental health data.

This project was independently completed as part of a personal data analytics portfolio, combining a background in Biochemistry and Molecular Biology with practical data skills.

---

## Dataset Description

| Field | Detail |
|---|---|
| Source | ASD Screening Dataset (publicly available) |
| Records | 704 patients |
| Variables | 21 columns |
| Target variable | `Class/ASD` — YES (positive) / NO (negative) |

### Key columns

| Column | Description |
|---|---|
| `A1_Score` to `A10_Score` | 10 behavioural screening questions (0 or 1 each) |
| `result` | Total screening score (0–10) |
| `gender` | Patient gender (m / f) |
| `age` | Patient age in years |
| `jundice` | Whether the patient had jaundice at birth (yes / no) |
| `austim` | Family history of autism (yes / no) |
| `contry_of_res` | Country of residence |
| `Class/ASD` | Final screening diagnosis (YES / NO) |

---

## Research Questions

1. How well does the screening tool distinguish between ASD and non-ASD individuals?
2. Does gender influence ASD screening positive rates?
3. Is jaundice at birth associated with a higher ASD screening rate?
4. Does family history of autism predict a higher likelihood of positive screening?
5. Which individual screening questions best discriminate between the two groups?

---

## Tools & Methods

- **SQL** — Data exploration, aggregation, and group comparison using SQLiteOnline (browser-based)
- **Microsoft Excel** — Dashboard creation, COUNTIFS risk factor analysis, and chart visualisation

---

## SQL Queries

All queries were run in [SQLiteOnline.com](https://sqliteonline.com) after importing the CSV.

### 1. Dataset overview
```sql
SELECT [Class/ASD] AS diagnosis,
       COUNT(*) AS total,
       ROUND(COUNT(*) * 100.0 / (SELECT COUNT(*) FROM autism), 1) AS percentage
FROM autism
GROUP BY [Class/ASD];
```

### 2. ASD rate by gender
```sql
SELECT gender,
       COUNT(*) AS total,
       SUM(CASE WHEN [Class/ASD] = 'YES' THEN 1 ELSE 0 END) AS asd_positive,
       ROUND(SUM(CASE WHEN [Class/ASD] = 'YES' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 1) AS asd_rate_pct
FROM autism
GROUP BY gender;
```

### 3. Jaundice at birth vs ASD outcome
```sql
SELECT jundice,
       COUNT(*) AS total,
       SUM(CASE WHEN [Class/ASD] = 'YES' THEN 1 ELSE 0 END) AS asd_count,
       ROUND(SUM(CASE WHEN [Class/ASD] = 'YES' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 1) AS asd_rate_pct
FROM autism
GROUP BY jundice;
```

### 4. Family history vs ASD outcome
```sql
SELECT austim AS family_history_autism,
       COUNT(*) AS total,
       SUM(CASE WHEN [Class/ASD] = 'YES' THEN 1 ELSE 0 END) AS asd_positive,
       ROUND(SUM(CASE WHEN [Class/ASD] = 'YES' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 1) AS asd_rate_pct
FROM autism
GROUP BY austim;
```

### 5. Average screening score by diagnosis
```sql
SELECT [Class/ASD] AS diagnosis,
       ROUND(AVG(result), 2) AS avg_score,
       MIN(result) AS min_score,
       MAX(result) AS max_score
FROM autism
GROUP BY [Class/ASD];
```

### 6. Individual question performance by diagnosis group
```sql
SELECT [Class/ASD],
       ROUND(AVG(A1_Score), 2) AS A1,
       ROUND(AVG(A2_Score), 2) AS A2,
       ROUND(AVG(A3_Score), 2) AS A3,
       ROUND(AVG(A4_Score), 2) AS A4,
       ROUND(AVG(A5_Score), 2) AS A5,
       ROUND(AVG(A6_Score), 2) AS A6,
       ROUND(AVG(A7_Score), 2) AS A7,
       ROUND(AVG(A8_Score), 2) AS A8,
       ROUND(AVG(A9_Score), 2) AS A9,
       ROUND(AVG(A10_Score), 2) AS A10
FROM autism
GROUP BY [Class/ASD];
```

### 7. Top countries by ASD diagnosis rate
```sql
SELECT contry_of_res AS country,
       COUNT(*) AS total_screened,
       SUM(CASE WHEN [Class/ASD] = 'YES' THEN 1 ELSE 0 END) AS asd_cases,
       ROUND(SUM(CASE WHEN [Class/ASD] = 'YES' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 1) AS asd_rate_pct
FROM autism
GROUP BY contry_of_res
HAVING COUNT(*) >= 10
ORDER BY asd_rate_pct DESC
LIMIT 10;
```

---

## Key Findings

**1. Screening tool validity**
ASD-positive participants scored an average of **8.3 out of 10** on the behavioural screening tool, compared to **3.6** for non-ASD participants — a gap of 4.7 points. This confirms the tool's strong ability to discriminate between positive and negative cases.

**2. Gender patterns**
Female participants showed a higher ASD screening positive rate (**30.6%**, n=337) compared to male participants (**23.4%**, n=367). This contrasts with traditional clinical diagnosis ratios where males are diagnosed more frequently, and may reflect growing awareness of ASD presentation in females or self-referral patterns in this adult screening dataset.

**3. Family history as a risk factor**
Participants with a family history of autism had an ASD positive screening rate of **47.3%** (n=91), compared to **23.8%** among those without a family history (n=613) — nearly double the rate. This finding is consistent with established evidence on the heritability of ASD.

**Bonus — Strongest individual screening questions**
Questions A5, A6, and A9 showed the largest score gap between ASD and non-ASD groups (gap of 0.60, 0.60, and 0.67 respectively), identifying them as the strongest individual behavioural predictors in the tool.

---

## Excel Dashboard

The Excel dashboard includes:
- 4 headline KPI boxes (total screened, ASD positive count, average score ASD, average score non-ASD)
- Bar chart: ASD diagnosis rate by gender
- Grouped bar chart: Average score per question (ASD vs non-ASD)
- Risk factor summary table (COUNTIFS — jaundice and family history vs ASD outcome)
- Written findings summary

> Screenshot of the dashboard: *(add your screenshot here after uploading)*

---

## Project Structure

```
autism-screening-analysis/
│
├── Autism_Screening.csv          # Raw dataset
├── autism_queries.sql            # All 7 SQL queries
├── ASD_Dashboard.xlsx            # Excel dashboard file
├── dashboard_screenshot.png      # Dashboard screenshot
└── README.md                     # This file
```

---

## About the Author

Ayooluwa Adekunle — Biochemistry & Molecular Biology student at Obafemi Awolowo University, Nigeria. Healthcare Data Analyst 

[LinkedIn](https://www.linkedin.com/in/ayooluwa-adekunle-baaab723a/) · [GitHub](https://github.com/)  

EXECL SPREADSHEET (https://1drv.ms/x/c/8ba1805a36e58169/IQDckrQbZ-WHSI0c6JVnf6G0AWala4nE86FggwlrfiF24mk?e=pYG5LU)
