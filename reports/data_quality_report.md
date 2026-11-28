# Olist E-commerce Analysis

## Data Quality and Validation Report

## 1. Project Overview

This report documents the data quality assessment, validation checks,
and exploratory analysis performed on the Olist Brazilian E-Commerce
Public Dataset.

The dataset contains historical e-commerce transactions from 2016--2018.
The purpose of this investigation is to identify missing values,
duplicates, inconsistent timestamps, and potential data issues before
using the data for business analysis.

## 2. Dataset Overview

  Dataset         Records   Columns
  ------------- --------- ---------
  Orders           99,441         8
  Customers        99,441         5
  Order Items     112,650         7
  Payments        103,886         5
  Reviews          99,224         7
  Products         32,951         9
  Sellers           3,095         4
  Categories           71         2

No duplicate rows were detected during the initial inspection of these
datasets.

## 3. Missing Value Analysis

### 3.1 Orders Dataset

  Column                            Missing Values   Percentage
  ------------------------------- ---------------- ------------
  order_approved_at                            160        0.16%
  order_delivered_carrier_date               1,783        1.79%
  order_delivered_customer_date              2,965        2.98%

Total missing cells: **4,908**.

Missing timestamps were investigated by order status. Many missing
values correspond to orders that have not reached the relevant delivery
stage.

### 3.2 Reviews Dataset

  Column                     Missing Values   Percentage
  ------------------------ ---------------- ------------
  review_comment_title               87,656       88.34%
  review_comment_message             58,247       58.70%

Review scores and identifiers have no missing values.

**Decision:** Missing review text will be retained. Customers can submit
a rating without providing written feedback, so these missing values do
not necessarily indicate invalid records.

### 3.3 Products Dataset

A total of 2,448 missing cells were identified during the initial
inspection. Further column-level investigation is required before
deciding how to handle these values.

## 4. Order Status Analysis

  Order Status      Count   Percentage
  -------------- -------- ------------
  delivered        96,478       97.02%
  shipped           1,107        1.11%
  canceled            625        0.63%
  unavailable         609        0.61%
  invoiced            314        0.32%
  processing          301        0.30%
  created               5        0.01%
  approved              2       0.002%

Approximately 97.02% of orders have delivered status. For the primary
completed-sales analysis, delivered orders are used to avoid including
canceled or unfinished transactions.

## 5. Date Validation

Order timestamp columns were converted into datetime format using
Pandas.

  Validation Check                              Invalid Records
  ------------------------------------------- -----------------
  Approval before purchase                                    0
  Carrier handover before approval                        1,359
  Customer delivery before carrier handover                  23

### 5.1 Carrier Date Anomalies

A total of 1,359 records have carrier timestamps earlier than approval
timestamps.

-   1,350 records belong to delivered orders.
-   9 records belong to shipped orders.
-   The maximum timestamp difference is approximately 4,109 hours (171
    days).

These records require special attention when analyzing individual
delivery stages.

### 5.2 Delivery Date Anomalies

A total of 23 delivered orders have customer delivery timestamps earlier
than carrier handover timestamps. These records should be flagged when
calculating delivery-stage durations.

**Decision:** Original records will be preserved. Anomalies will be
flagged or excluded only where they affect the validity of a particular
metric.

## 6. Delivery Time Analysis

Delivery duration was calculated using:

`order_delivered_customer_date - order_purchase_timestamp`

The analysis uses delivered orders with available delivery timestamps.

  Metric              Delivery Time
  ----------------- ---------------
  Average                12.56 days
  Median                 10.22 days
  25th Percentile         6.77 days
  75th Percentile        15.72 days
  90th Percentile        23.10 days
  95th Percentile        29.27 days
  99th Percentile        46.05 days
  Maximum               209.63 days

### Interpretation

-   The average delivery time is approximately 12.56 days.
-   The median is lower than the average, indicating a right-skewed
    distribution.
-   Most deliveries occur within a relatively shorter period, while a
    smaller number of orders take considerably longer.
-   Extreme delivery durations should be investigated rather than
    automatically removed.

## 7. Exploratory Data Analysis

### 7.1 Revenue Calculation Methodology

The sales analysis uses the following approach:

1.  Filter orders where `order_status = 'delivered'`.
2.  Merge order items with delivered orders using `order_id`.
3.  Calculate product revenue using the `price` column.
4.  Aggregate revenue and unique orders by month or product category.

**Important:** The order-items dataset contains multiple rows per order.
Therefore, the number of product rows must not be treated as the number
of orders.

Freight charges are excluded from the primary product revenue
calculation.

### 7.2 Monthly Revenue and Order Analysis

Monthly revenue is calculated as:

`Monthly Revenue = Sum of Item Price`

Monthly order volume is calculated as:

`Monthly Orders = Number of Unique Order IDs`

Key findings:

-   November 2017 recorded the highest monthly revenue, approximately
    R\$987,765.
-   November 2017 also recorded the highest monthly order volume, with
    7,289 unique orders.
-   Revenue decreased to approximately R\$726,033 in December 2017.
-   Order volume decreased to 5,513 in December 2017.

This indicates that the December revenue decline was primarily
associated with fewer orders, alongside a smaller decrease in average
order value.

### 7.3 Average Order Value (AOV)

Average Order Value is calculated as:

`AOV = Monthly Revenue / Monthly Unique Orders`

Selected observations:

  Month             AOV (BRL)
  --------------- -----------
  November 2017        135.51
  December 2017        131.69

The highest observed monthly AOV was approximately R\$152.17 in October
2016. However, this month had only 265 orders, so it should not be
treated as directly comparable to high-volume months.

### 7.4 Product Category Revenue Analysis

Product information and English category translations were joined to
delivered order items.

The resulting product-sales dataset contains **110,197 rows and 23
columns**.

A left join was used to preserve delivered order-item records even when
product-category information was unavailable.

Top five categories by revenue:

  Product Category            Revenue (BRL)   Unique Orders
  ------------------------- --------------- ---------------
  Health & Beauty              1,233,131.72           8,647
  Watches & Gifts              1,166,176.98           5,495
  Bed, Bath & Table            1,023,434.76           9,272
  Sports & Leisure               954,852.55           7,530
  Computers & Accessories        888,724.61           6,530

Health & Beauty currently leads the category revenue analysis.

A total of 1,559 rows have missing English category names. The
investigation found that 1,537 rows have a missing original
`product_category_name`, while 22 rows have an original category but no
English translation:

-   `portateis_cozinha_e_preparadores_de_alimentos`: 14 rows
-   `pc_gamer`: 8 rows

These records should be retained in the analysis and assigned a
reporting label such as `Unknown / untranslated` when a complete
category summary is required. The original category columns should
remain unchanged.

### 7.5 Customer Repeat-Purchase Analysis

Customer-level repeat purchasing was analyzed using
`customer_unique_id`, rather than `customer_id`, because the unique
customer identifier is used to identify the same customer across orders.

The observed frequency distribution is:

    Delivered Orders per Customer   Customers
  ------------------------------- -----------
                                1      90,557
                                2       2,573
                                3         181
                                4          28
                                5           9
                                6           5
                                7           3
                                9           1
                               15           1

Based on this frequency distribution:

-   Total unique customers: **93,359**
-   One-time customers: **90,557**
-   Repeat customers: **2,802**
-   Repeat-customer share: approximately **3.00%**

Most repeat customers placed two delivered orders. Customers with very
high order counts were rare.

**Interpretation:** Repeat purchasing was uncommon within the observed
historical dataset. This should not be treated as a complete long-term
retention rate because the dataset covers a limited period and customers
had different lengths of time in which to make another purchase.

### 7.6 Customer-Level Revenue Analysis

Customer revenue was calculated by linking delivered order items to the
customer table using `customer_id`, then grouping by
`customer_unique_id`.

The customer-level metrics include:

-   **Total spending:** Sum of item `price` across a customer's
    delivered orders.
-   **Total orders:** Number of unique delivered `order_id` values for
    that customer.
-   **Average order value per customer:** Total spending divided by the
    customer's number of delivered orders.

The inspected highest-revenue customer had relatively few orders and a
high average order value. This illustrates that a customer's total
revenue can be driven by larger spending per order, not only by frequent
purchases.

**Important distinction:** Customer-level average order value is not the
same as monthly overall AOV. It is calculated separately for each
customer using that customer's delivered orders.

Revenue concentration among the top 10 customers has not yet been
verified and is not reported as a finding in this version.

## 8. Data Quality Decisions

The following decisions guide the analysis:

1.  Retain missing review comments without artificial imputation.
2.  Do not blindly fill missing order timestamps.
3.  Preserve timestamp anomalies in the original dataset.
4.  Flag invalid timestamp sequences during delivery-stage analysis.
5.  Avoid deleting valid sales records solely because of delivery
    timestamp anomalies.
6.  Use delivered orders for the primary completed-sales analysis.
7.  Aggregate data at the correct table grain before joining tables.
8.  Use unique order IDs when calculating order counts.
9.  Retain records with missing category mappings and label them
    explicitly for reporting.
10. Do not interpret revenue as profit because product cost information
    is not available.

## 9. Conclusion

The initial data quality investigation identified missing timestamps,
optional review text, and inconsistent delivery-stage timestamps.

The exploratory analysis has established an initial understanding of
monthly sales performance, average order value, product-category
revenue, repeat purchasing, and customer-level spending.

The dataset is suitable for further business analysis, provided that the
identified anomalies and differences in table granularity are handled
carefully.

## 10. Next Steps

-   Analyze geographic sales performance.
-   Investigate delivery performance.
-   Analyze customer review scores and satisfaction.
-   Quantify customer revenue concentration.
-   Perform SQL-based business analysis.
-   Develop an interactive Power BI dashboard.
-   Prepare final business insights and recommendations.
