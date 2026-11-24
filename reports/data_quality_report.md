# Olist E-commerce Analysis
## Data Quality and Validation Report

### 1. Project Overview

This report documents the initial data inspection, missing-value analysis, and validation checks performed on the Olist Brazilian E-Commerce Public Dataset.

The dataset contains historical e-commerce transactions from 2016–2018.

### 2. Dataset Overview

| Dataset | Records | Columns |
|---|---:|---:|
| Orders | 99,441 | 8 |
| Customers | 99,441 | 5 |
| Order Items | 112,650 | 7 |
| Payments | 103,886 | 5 |
| Reviews | 99,224 | 7 |
| Products | 32,951 | 9 |
| Sellers | 3,095 | 4 |
| Categories | 71 | 2 |

No duplicate rows were detected in the initial inspection of these datasets.

### 3. Missing Value Analysis

#### Orders Dataset

| Column | Missing Values | Percentage |
|---|---:|---:|
| order_approved_at | 160 | 0.16% |
| order_delivered_carrier_date | 1,783 | 1.79% |
| order_delivered_customer_date | 2,965 | 2.98% |

Total missing cells: 4,908.

Missing timestamps were investigated by order status. Many missing values correspond to orders that have not reached the relevant delivery stage.

#### Reviews Dataset

| Column | Missing Values | Percentage |
|---|---:|---:|
| review_comment_title | 87,656 | 88.34% |
| review_comment_message | 58,247 | 58.70% |

Review scores and identifiers have no missing values.

**Decision:** Missing review text will be retained because customers can submit a rating without providing written feedback.

#### Products Dataset

A total of 2,448 missing cells were identified during initial inspection. Further column-level investigation may be performed before deciding how to handle them.

### 4. Order Status Analysis

| Order Status | Count |
|---|---:|
| delivered | 96,478 |
| shipped | 1,107 |
| canceled | 625 |
| unavailable | 609 |
| invoiced | 314 |
| processing | 301 |
| created | 5 |
| approved | 2 |

Approximately 97.02% of orders have delivered status.

### 5. Date Validation

Order timestamp columns were converted into datetime format.

Validation results:

| Validation Check | Invalid Records |
|---|---:|
| Approval before purchase | 0 |
| Carrier handover before approval | 1,359 |
| Customer delivery before carrier handover | 23 |

#### Carrier Date Anomalies

Out of 1,359 records with carrier dates earlier than approval dates:

- 1,350 belong to delivered orders.
- 9 belong to shipped orders.
- The maximum timestamp difference is approximately 4,109 hours (171 days).

These records require special attention in delivery-stage analysis.

#### Delivery Date Anomalies

23 delivered orders have customer delivery timestamps earlier than carrier handover timestamps.

### 6. Delivery Time Analysis

Delivery duration was calculated using:

`order_delivered_customer_date - order_purchase_timestamp`

Observed statistics for the delivered-order subset:

| Metric | Delivery Time |
|---|---:|
| Average | 12.56 days |
| Median | 10.22 days |
| 90th Percentile | 23.10 days |
| 95th Percentile | 29.27 days |
| 99th Percentile | 46.05 days |
| Maximum | 209.63 days |

The distribution is right-skewed, indicating that a relatively small number of orders took substantially longer to arrive.

### 7. Data Quality Decisions

