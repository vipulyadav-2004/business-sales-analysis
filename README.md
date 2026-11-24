# Brazilian E-Commerce Business Analysis

An end-to-end business analytics project using the Olist Brazilian E-Commerce Public Dataset to investigate sales performance, customer behaviour, product trends, and delivery operations.

## Project Overview

This project analyzes historical e-commerce transactions from Olist, a Brazilian online marketplace.

The goal is to transform raw transactional data into meaningful business insights using Python, SQL, and Power BI.

## Business Questions

- How do monthly sales and order volumes change over time?
- Which product categories generate the highest sales?
- What is the average order value?
- Which locations contribute most to sales?
- How long does delivery take?
- How are delivery experiences associated with customer review scores?
- How frequently do customers make repeat purchases?

## Tools and Technologies

| Tool | Purpose |
|---|---|
| Python | Data cleaning and analysis |
| Pandas | Data manipulation |
| NumPy | Numerical operations |
| Matplotlib / Seaborn | Data visualization |
| SQL | Business analysis and querying |
| Power BI | Interactive dashboard development |
| GitHub | Version control and project documentation |

## Project Structure

```text
business-sales-analysis/
│
├── notebooks/
│   └── 01_data_understanding.ipynb
│
├── reports/
│   └── data_quality_report.md
│
├── sql/
├── powerbi/
├── data/
│   ├── raw/
│   └── processed/
│
├── .gitignore
└── README.md
```

Raw and processed datasets are excluded from Git tracking.

## Current Progress

- [x] Dataset loading and initial inspection
- [x] Missing-value investigation
- [x] Duplicate checks
- [x] Date conversion and validation
- [x] Timestamp anomaly investigation
- [x] Initial data quality documentation
- [ ] Exploratory Data Analysis
- [ ] SQL business analysis
- [ ] Power BI dashboard
- [ ] Final business insights

## Data Quality Highlights

- 99,441 orders inspected.
- No duplicate rows detected in the initial dataset inspection.
- Missing order timestamps investigated.
- Review comment missingness analyzed.
- 1,359 carrier-date sequence anomalies identified.
- 23 delivery-date sequence anomalies identified.

See [Data Quality Report](reports/data_quality_report.md) for details.

## Dataset

Olist Brazilian E-Commerce Public Dataset.

The dataset contains historical transactions from 2016–2018.

## Project Objective

The final objective is to build a complete business analytics case study that demonstrates practical skills in data cleaning, exploratory analysis, SQL, visualization, and communicating business recommendations.

---

**Project Status:** In Progress