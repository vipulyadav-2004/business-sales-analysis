# Olist E-commerce Analysis
## Initial Data Quality Report

### Dataset Overview
- Dataset: Olist Brazilian E-Commerce Public Dataset
- Orders records: 99,441
- Orders columns: 8
- Duplicate rows detected: 0

### Missing Value Findings
- order_approved_at: 160 missing values
- order_delivered_carrier_date: 1,783 missing values
- order_delivered_customer_date: 2,965 missing values
- Total missing cells: 4,908

### Order Status Findings
- Delivered: 96,478
- Shipped: 1,107
- Cancelled: 625
- Unavailable: 609
- Invoiced: 314
- Processing: 301
- Created: 5
- Approved: 2

### Initial Data Quality Issues
1. Missing timestamps in three date columns.
2. Date columns are stored as string/object datatypes.
3. Order status contains multiple lifecycle stages.

### Next Steps
1. Investigate missing timestamps by order status.
2. Convert date columns into datetime format.
3. Check duplicate order IDs.
4. Validate date sequences.
5. Document all cleaning decisions.