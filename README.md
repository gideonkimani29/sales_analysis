# Northwind Analytics — End-to-End SQL Portfolio Project

A structured SQL analytics project built on the classic Northwind database using SQLite and Python.
The project follows a layered data modelling approach — staging → marts → analysis —
covering four business tracks: sales, customers, products, and employee performance.

---

## Tools & Stack

| Layer | Tool |
|---|---|
| Database | SQLite (Northwind.db) |
| Query language | SQL (SQLite dialect) |
| Notebook | Jupyter (pandas, sqlalchemy) |
| Visualisation | matplotlib / pandas plotting |

---

## Project Structure

```
northwind_analytics/
│
├── Northwind.db                  # Source database
├── northwind_analytics.ipynb     # Main notebook (all layers + analysis)
└── README.md                     # This file
```

---

## Schema Overview

The Northwind database models a fictional trading company with 11 tables.

```
Customers ──────────┐
Employees ──────────┼──► Orders ──► OrderDetails ──► Products ──► Categories
Shippers  ──────────┘                                          └──► Suppliers

Employees ──► EmployeeTerritories ──► Territories ──► Regions
```

| Table | Rows | Description |
|---|---|---|
| Customers | 91 | Companies across 21 countries |
| Orders | 830 | Customer orders with shipping info |
| OrderDetails | 2,155 | Line items — product, quantity, price, discount |
| Products | 77 | Catalogue across 8 categories |
| Categories | 8 | Product groupings |
| Suppliers | 29 | Suppliers across 16 countries |
| Employees | 9 | Sales reps with manager hierarchy |
| Shippers | 3 | Shipping companies |
| Territories | 53 | Sales territories |
| Regions | 4 | Sales regions |

---

## Layered Modelling Approach

### 1 — Staging Layer

Clean views on top of raw tables. No logic, no aggregations — just renaming and light casting.

| View | Source Table | Purpose |
|---|---|---|
| `stg_customers` | Customers | Standardised customer fields |
| `stg_orders` | Orders | Cleaned dates, renamed keys |
| `stg_order_details` | OrderDetails | Added `line_total` calculation |
| `stg_products` | Products | Renamed fields, cast discontinued flag |
| `stg_employees` | Employees | Concatenated name, cleaned hire date |

### 2 — Mart Layer

Business-ready aggregations. Intermediate joins are inlined directly due to SQLite view-on-view limitations.

| View | Grain | Key Metrics |
|---|---|---|
| `mart_sales_monthly` | Month | Revenue, order count, avg order value |
| `mart_customer_rfm` | Customer | Recency, frequency, monetary value |
| `mart_product_performance` | Product | Units sold, revenue, avg discount |
| `mart_employee_performance` | Employee | Revenue, orders, avg days to ship |

---

## Analysis Tracks

### Track 1 — Sales & Revenue
- Monthly and yearly revenue trends
- Revenue by country
- Year-over-year comparison

### Track 2 — Customer Behavior
- Top customers by lifetime value
- RFM segmentation (Champions / Loyal / Promising / At Risk / Lost)
- Segment-level revenue contribution

### Track 3 — Product & Category Performance
- Top products and categories by revenue
- Discontinued vs active product revenue
- Supplier contribution

### Track 4 — Employee Performance
- Sales rep leaderboard by revenue
- Revenue per employee by year
- Shipper speed comparison

---

## Key Findings


| # | Finding | Track |
|---|---|---|
| 1 | Revenue peaked in **[month/year]**, driven by **[category]** | Sales |
| 2 | Top 10 customers account for 616,789.20 of total revenue | Customers |
| 3 | **X%** of customers are classified as At Risk or Lost | Customers |
| 4 | **Cemembert Pierot** is the highest-revenue product; **Dairy**  leads by category | Products |
| 5 | Discontinued products still contributed **$X** in revenue | Products |
| 6 | **Peacock, Magaret** leads in total revenue; **Buchanan Steven** has the fastest ship time | Employees |
| 7 | **[Shipper]** averages the fastest delivery at **X days** | Operations |

---

## How to Run

1. Clone or download this repository
2. Ensure `Northwind.db` is in the same folder as the notebook
3. Install dependencies:
   ```bash
   pip install pandas sqlalchemy jupyter
   ```
4. Launch the notebook:
   ```bash
   jupyter notebook northwind_analytics.ipynb
   ```
5. Run all cells top to bottom

---

## Notes

- All monetary values are in USD
- The dataset covers orders from **1996 to 1998**
- SQLite does not reliably support view-on-view references, so intermediate joins are inlined into mart views
- `line_total` is calculated as `UnitPrice × Quantity × (1 - Discount)`

---

*Project by Gideon Kimani · ALX Africa — Data Analytics & Aspiring Data Engineering*
