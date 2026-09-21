# Brazilian E-Commerce Analytics

**Author:** Nithin Chillara  
**Dataset:** Olist Brazilian E-Commerce Public Dataset (https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) 
**Period:** September 2016 – October 2018  
**Technology:** Python | Pandas | NumPy | Matplotlib | Seaborn | SQLite (SQL Analysis)

---

## Overview

A comprehensive end-to-end data analysis of the Olist Brazilian e-commerce dataset. This project covers data cleaning, preprocessing, exploratory data analysis (EDA), KPI tracking, time series analysis, customer segmentation (RFM), geographic analysis, payment analysis, review analysis, retention cohorts, **SQL analysis (15 queries via SQLite)**, and 27 answered business questions — all backed by real data with no fabricated results.

---

## Submission Files (4 Files Only)

| # | File | Description |
|---|------|-------------|
| 1 | `Nithin Chillara_BrazilianECommerceAnalytics.ipynb` | Complete executable Python analysis notebook |
| 2 | `requirements.txt` | Python library dependencies |
| 3 | `Nithin Chillara_ProjectReport.docx` | Full Word project report with SQL section |
| 4 | `README.md` | This file — setup, dataset, structure, running instructions |

---

## Dataset

The [Olist Brazilian E-Commerce Public Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) — nine CSV files:

| File | Rows | Description |
|------|------|-------------|
| `olist_customers_dataset.csv` | 99,441 | Customer IDs, zip codes, cities, states |
| `olist_orders_dataset.csv` | 99,441 | Order IDs, statuses, timestamps |
| `olist_order_items_dataset.csv` | 112,650 | Item-level price, freight, seller, product |
| `olist_order_payments_dataset.csv` | 103,886 | Payment type, value, installments |
| `olist_order_reviews_dataset.csv` | 99,224 | Review scores, dates, comments |
| `olist_products_dataset.csv` | 32,951 | Product categories, dimensions, weight |
| `olist_sellers_dataset.csv` | 3,095 | Seller IDs, zip codes, states |
| `olist_geolocation_dataset.csv` | 1,000,163 | Zip code lat/lng coordinates |
| `product_category_name_translation.csv` | 71 | Portuguese → English category names |

> All CSV files must be in the **same directory** as the notebook.

---

## Setup

### Install Python dependencies

```bash
pip install -r requirements.txt
```

**requirements.txt contents:**
```
pandas>=2.0.0
numpy>=1.24.0
matplotlib>=3.7.0
seaborn>=0.12.0
jupyter>=1.0.0
nbconvert>=7.0.0
```

> `sqlite3` is part of Python's standard library — no additional installation needed for SQL analysis.

---

## Running the Notebook

### Option A — Jupyter Notebook (interactive)
```bash
jupyter notebook "Nithin Chillara_BrazilianECommerceAnalytics.ipynb"
```
Select **Kernel → Restart & Run All** to execute all cells from scratch.

### Option B — Command line (batch execution)
```bash
jupyter nbconvert --to notebook --execute --inplace \
  "Nithin Chillara_BrazilianECommerceAnalytics.ipynb"
```

### Option C — JupyterLab
```bash
jupyter lab
```

---

## Notebook Structure

| Section | Contents |
|---------|----------|
| 1. Setup & Imports | Libraries, display settings |
| 2. Data Loading | Load all 9 CSV files |
| 3. Data Cleaning & Preprocessing | Date parsing, null handling, aggregation strategy, master table |
| 4. EDA | Missing values, distributions, order status |
| 5. Descriptive Statistics | Summary stats, correlation matrix |
| 6. KPI Analysis | Revenue, orders, customers, AOV, review score, delivery time |
| 7. Sales Analysis | Monthly revenue (Q1), category revenue (Q2), AOV (Q3), day-of-week (Q4), buckets (Q5) |
| 8. Time Series Analysis | Hourly (Q6), YoY growth (Q7), rolling average (Q8), quarterly (Q9) |
| 9. Customer Analysis | Frequency (Q10), spend distribution (Q11) |
| 10. RFM Segmentation | Recency-Frequency-Monetary scoring, 6 customer segments |
| 11. Geographic Analysis | State revenue (Q12), freight by state (Q13), seller distribution (Q14) |
| 12. Review Analysis | Score distribution (Q15), delivery delay impact (Q16), category scores (Q17) |
| 13. Payment Analysis | Method share (Q18), installments (Q19), installment vs value (Q20) |
| 14. Retention Analysis | Monthly cohort retention heatmap |
| 15. Seller Analysis | Top sellers (Q21), seller reviews (Q22) |
| 16. Advanced Analysis | Freight ratio (Q23), price vs review (Q24), cancellations (Q25), dimensions (Q26), late deliveries (Q27) |
| **17. SQL Analysis** | **15 SQL queries via SQLite — see details below** |

---

## SQL Analysis Section (Section 17)

The SQL section creates an **in-memory SQLite database** from the CSV data and runs 15 real SQL queries. No external database is needed.

### How it works
```python
import sqlite3
conn = sqlite3.connect(':memory:')          # in-memory database
df.to_sql('orders', conn, ...)              # load CSV data as SQL table
result = pd.read_sql_query(sql, conn)       # execute SQL, get DataFrame
```

### SQL Queries

| Query | Title | SQL Features |
|-------|-------|-------------|
| SQL Q1 | Overall KPIs | Multi-table JOIN, SUM, COUNT DISTINCT |
| SQL Q2 | Top 10 categories by revenue | LEFT JOIN, COALESCE, GROUP BY, LIMIT |
| SQL Q3 | Monthly revenue trend | SUBSTR() date extraction, HAVING |
| SQL Q4 | Top 10 states by revenue | GROUP BY state, aggregates |
| SQL Q5 | Payment method breakdown | JOIN payments, GROUP BY payment_type |
| SQL Q6 | Review score by delivery delay | CASE WHEN, JULIANDAY() arithmetic |
| SQL Q7 | Top 10 sellers | 3-table JOIN, ORDER BY revenue |
| SQL Q8 | Customer frequency distribution | Nested subquery |
| SQL Q9 | Quarterly revenue 2017 vs 2018 | CASE WHEN quarter classification |
| SQL Q10 | Delivery days + late % by state | JULIANDAY, CASE WHEN late |
| SQL Q11 | Review scores by category | 4-table JOIN, HAVING filter |
| SQL Q12 | Cumulative revenue (running total) | Self-join for running sum |
| SQL Q13 | Order status distribution | Window function: `SUM(COUNT(*)) OVER ()` |
| SQL Q14 | Freight-to-price ratio | Arithmetic expression in SELECT |
| SQL Q15 | High-value repeat customers | HAVING order_count>1 AND spend>500 |

### Key SQL Results (Actual Data)

**SQL Q1 — Overall KPIs:**
```
total_orders: 96,478 | unique_customers: 93,358
total_product_revenue: R$ 13,221,498.11
total_gmv: R$ 15,419,773.75 | avg_order_value: R$ 139.93
```

**SQL Q6 — Delivery Delay vs Review Score:**
```
Very Early (>10d early):   avg_score = 4.32 (52,607 reviews)
Early (1-10d early):       avg_score = 4.25 (34,589 reviews)
Exactly On Time:           avg_score = 4.10  (2,748 reviews)
Slight Delay (1-7d late):  avg_score = 2.71  (3,612 reviews)
Severe Delay (>7d late):   avg_score = 1.70  (2,797 reviews)
```

**SQL Q8 — Customer Frequency:**
```
1 order:  90,557 customers (97.00%)
2 orders:  2,573 customers ( 2.76%)
3+ orders:   228 customers ( 0.24%)
```

---

## Key KPIs (Actual Data)

| KPI | Value |
|-----|-------|
| Total Product Revenue | R$ 13,221,498 |
| Total GMV (incl. freight) | R$ 15,419,774 |
| Delivered Orders | 96,478 |
| Unique Customers | 93,358 |
| Avg Order Value | R$ 159.83 |
| Avg Review Score | 4.16 / 5 |
| Avg Delivery Time | 12.1 days |
| Active Sellers | 3,095 |
| Repeat Customer Rate | 3.12% |
| Top Category | health_beauty (R$1.23M) |
| Top Payment Method | Credit card (73.9%) |
| Top State | SP — R$5.07M |

---

## Data Integrity Notes

- **No revenue duplication:** `order_items` and `payments` aggregated to order level before any join.
- **One review per order:** Deduplicated by keeping earliest review_creation_date per order_id.
- **Safe SQL joins:** All SQL queries join via order-level keys; item-level tables are aggregated first where needed.
- **Delivered orders only:** Revenue KPIs use `WHERE order_status = 'delivered'`.
- **No fabricated data:** Every chart, stat, and SQL result comes from actual dataset execution.

---

*Brazilian E-Commerce Analytics — Nithin Chillara*
