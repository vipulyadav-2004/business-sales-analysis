# Brazilian E-Commerce Business Analysis

An end-to-end business analytics project using the **Olist Brazilian E-Commerce Public Dataset** to investigate sales performance, product trends, customer behaviour, and delivery operations.

## Project Overview

This project analyzes historical e-commerce transactions from Olist, a Brazilian online marketplace.

The objective is to transform raw transactional data into meaningful business insights using Python, SQL, and Power BI.

The project follows a practical business analytics workflow:

1. Data understanding and quality assessment
2. Data cleaning and validation
3. Exploratory Data Analysis (EDA)
4. SQL-based business analysis
5. Interactive dashboard development
6. Business insights and recommendations

## Business Questions

- How do monthly revenue and order volumes change over time?
- Which product categories generate the highest revenue?
- What is the average order value (AOV)?
- Which locations contribute most to sales?
- How long does delivery take?
- How are delivery experiences associated with customer review scores?
- How frequently do customers make repeat purchases?

## Tools and Technologies

| Tool | Purpose |
|---|---|
| Python | Data cleaning and analysis |
| Pandas | Data manipulation and aggregation |
| NumPy | Numerical operations |
| Matplotlib / Seaborn | Data visualization |
| SQL | Business analysis and querying |
| Power BI | Interactive dashboard development |
| GitHub | Version control and documentation |

## Project Structure

```text
business-sales-analysis/
│
├── data/
│   ├── raw/
│   └── processed/
│
├── notebooks/
│   ├── 01_data_understanding.ipynb
│   └── 02_exploratory_data_analysis.ipynb
│
├── reports/
│   └── data_quality_report.md
│
├── sql/
├── powerbi/
├── .gitignore
└── README.md
```

Raw and processed datasets are excluded from Git tracking.

## Dataset

**Olist Brazilian E-Commerce Public Dataset**

The dataset contains historical e-commerce transactions from 2016–2018, including orders, customers, products, payments, reviews, sellers, and delivery information.

## Analysis Methodology

### Revenue Definition

For the primary sales analysis:

- Only orders with `order_status = 'delivered'` are included.
- Revenue is calculated using the `price` column from order items.
- Freight charges are excluded from product revenue.
- Monthly order volume counts unique `order_id` values.
- AOV is calculated as monthly revenue divided by unique monthly orders.

This approach avoids counting individual product rows as separate orders.

### Current EDA Findings

**1. Monthly Sales Performance**

- November 2017 recorded the highest monthly revenue in the analyzed period, approximately R$987,765.
- Monthly order volume also peaked in November 2017, with 7,289 unique orders.
- Revenue declined in December 2017, alongside a decrease in order volume.

**2. Average Order Value**

- November 2017 AOV was approximately R$135.51.
- December 2017 AOV was approximately R$131.69.
- The highest observed monthly AOV was approximately R$152.17 in October 2016, based on only 265 orders.

**3. Product Category Performance**

The leading product category by delivered-item revenue was:

| Category | Revenue (BRL) |
|---|---:|
| Health & Beauty | 1,233,131.72 |
| Watches & Gifts | 1,166,176.98 |
| Bed, Bath & Table | 1,023,434.76 |
| Sports & Leisure | 954,852.55 |
| Computers & Accessories | 888,724.61 |

These findings are based on the current category revenue aggregation. Category mapping and missing category values are still being investigated.

## Data Quality Highlights

- 99,441 orders inspected.
- No duplicate rows detected during initial dataset inspection.
- Missing order timestamps investigated by order status.
- Optional review-comment missingness identified.
- 1,359 carrier-date sequence anomalies identified.
- 23 delivery-date sequence anomalies identified.
- Delivery duration and its distribution analyzed.

For detailed findings, see [Data Quality Report](reports/data_quality_report.md).

## Project Progress

- [x] Dataset loading and initial inspection
- [x] Missing-value investigation
- [x] Duplicate checks
- [x] Date conversion and validation
- [x] Timestamp anomaly investigation
- [x] Data quality report
- [x] Monthly revenue analysis
- [x] Monthly order-volume analysis
- [x] Average order value analysis
- [x] Product-category revenue analysis
- [ ] Complete category missing-value investigation
- [ ] Geographic sales analysis
- [ ] Delivery performance analysis
- [ ] Customer review and satisfaction analysis
- [ ] Repeat customer analysis
- [ ] SQL business analysis
- [ ] Power BI dashboard
- [ ] Final business recommendations

## Project Objective

The final objective is to build a complete business analytics case study demonstrating practical skills in data cleaning, exploratory analysis, SQL, visualization, and business communication.

---

**Project Status:** In Progress