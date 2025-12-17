
# Contoso Sales Performance Analysis (PostgreSQL)

## Overview
This project analyzes Contoso’s global sales data using PostgreSQL to produce business-ready metrics and insights.  
The output of the SQL analysis feeds a sales performance dashboard covering revenue, profit, quantity sold, customer distribution, product performance, and geographical trends.

This is a pure analytics project. No machine learning. All results are derived from SQL queries.

---

## Objective
Provide a single source of truth for Contoso’s sales performance by answering:
- How much revenue and profit is the business generating?
- How are sales trending over time?
- Which product categories and countries drive the most revenue?
- How are customers distributed geographically?

The analysis is designed to support executive and commercial decision-making.

---

## Dataset
**Contoso Sales Dataset**

The dataset follows a star schema and contains transactional sales data with supporting dimensions.

### Fact Table
- `fact_sales`
  - `sales_amount`
  - `profit`
  - `quantity`
  - `order_date_key`
  - `customer_key`
  - `product_key`
  - `geography_key`

### Dimension Tables
- `dim_customer`
- `dim_product`
- `dim_category`
- `dim_date`
- `dim_geography`

All data is stored and queried in PostgreSQL.

---

## Data Model
The project uses a star schema to enable fast aggregation and slicing.

- Central fact table: `fact_sales`
- Dimensions connected via surrogate keys
- Optimized for time-series, category, customer, and geography analysis

This structure aligns with BI tools such as Power BI.

---

## Key Metrics
- Total Revenue
- Total Profit
- Total Quantity Sold
- Total Customers
- Year-over-Year Revenue Trend
- Sales by Product Category
- Sales by Country
- Customer Distribution by Continent

---

## SQL Analysis

### Total Revenue, Profit, and Quantity
```sql
SELECT
    SUM(sales_amount) AS total_revenue,
    SUM(profit) AS total_profit,
    SUM(quantity) AS total_quantity
FROM fact_sales;
````

### Customer Count

```sql
SELECT COUNT(DISTINCT customer_key) AS customer_count
FROM fact_sales;
```

### Sales by Year

```sql
SELECT
    d.year,
    SUM(f.sales_amount) AS revenue
FROM fact_sales f
JOIN dim_date d
    ON f.order_date_key = d.date_key
GROUP BY d.year
ORDER BY d.year;
```

### Top 5 Product Categories by Revenue

```sql
SELECT
    c.category_name,
    SUM(f.sales_amount) AS revenue
FROM fact_sales f
JOIN dim_product p
    ON f.product_key = p.product_key
JOIN dim_category c
    ON p.category_key = c.category_key
GROUP BY c.category_name
ORDER BY revenue DESC
LIMIT 5;
```

### Top 5 Countries by Revenue

```sql
SELECT
    g.country,
    SUM(f.sales_amount) AS revenue
FROM fact_sales f
JOIN dim_geography g
    ON f.geography_key = g.geography_key
GROUP BY g.country
ORDER BY revenue DESC
LIMIT 5;
```

### Customer Distribution by Continent

```sql
SELECT
    g.continent,
    COUNT(DISTINCT f.customer_key) AS customer_count
FROM fact_sales f
JOIN dim_geography g
    ON f.geography_key = g.geography_key
GROUP BY g.continent;
```

---

## Dashboard

The Power BI dashboard is built directly on top of the SQL outputs.

### Visuals Included

* KPI cards: Revenue, Profit, Quantity, Customers
* Line chart: Sales by Year
* Bar charts: Top Categories and Top Countries
* Donut chart: Customer Distribution by Continent
* Map: Geographical Sales Performance

Each visual maps one-to-one with a SQL query.

---

## Repository Structure

```
contoso-sales-performance/
│
├── sql/
│   ├── schema.sql
│   ├── data_loading.sql
│   ├── kpi_queries.sql
│   ├── sales_trends.sql
│   ├── product_analysis.sql
│   ├── geography_analysis.sql
│
├── dashboard/
│   └── contoso_sales_dashboard.pbix
│
├── images/
│   └── dashboard_preview.png
│
└── README.md
```

---

## Insights

* Revenue shows long-term growth with a noticeable peak followed by decline.
* A small number of product categories account for the majority of sales.
* North America contributes the largest share of revenue.
* Customer distribution is uneven across continents.

---

## Limitations

* Historical data only; no real-time ingestion.
* No forecasting or predictive analysis.
* Manual refresh required for dashboard updates.

---

## Tools Used

* PostgreSQL
* SQL (joins, aggregations, CTEs, date functions)
* Power BI (visualization)
* GitHub (version control and portfolio hosting)

---

## Use Case

This project demonstrates:

* Business-focused SQL analysis
* Dimensional data modeling
* KPI-driven dashboards
* Analytics engineering fundamentals

Suitable for Data Analyst, BI Analyst, and Analytics Engineer portfolios.


