# Azure Databricks E-Commerce Lakehouse (Medallion Architecture)

End-to-end data engineering project built on **Azure Databricks**, using **PySpark** and **Delta Lake** to transform raw e-commerce data into business-ready analytics — following the **Medallion Architecture** pattern (Bronze → Silver → Gold).

Built on the real-world [Olist Brazilian E-Commerce dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) (100K+ orders).

---

## Architecture

```
Raw CSV Files (Unity Catalog Volume)
        ↓
Bronze Layer — raw ingestion, Delta tables, no transformation
        ↓
Silver Layer — cleaned, deduplicated, joined into one analytics-ready table
        ↓
Gold Layer — business KPIs (revenue, categories, payments, geography)
        ↓
Visualizations / Dashboard
```

---

## Tech Stack

- **Azure Databricks** (Serverless compute)
- **PySpark**
- **Delta Lake**
- **Spark SQL**
- **Unity Catalog** (managed tables & volumes)

---

## Dataset

5 core files from the Olist dataset:

| File | Description |
|---|---|
| `olist_customers_dataset.csv` | Customer IDs and locations |
| `olist_orders_dataset.csv` | Order status and timestamps |
| `olist_order_items_dataset.csv` | Line items, price, freight |
| `olist_products_dataset.csv` | Product categories |
| `olist_order_payments_dataset.csv` | Payment type and value |

---

## Pipeline

### 1. Bronze Layer — `01_data_ingestion.ipynb`
Reads all 5 raw CSVs from a Unity Catalog Volume and writes them as managed Delta tables with no transformation, preserving an auditable raw copy of the source data.

**Tables created:** `bronze_customers`, `bronze_orders`, `bronze_order_items`, `bronze_products`, `bronze_payments`

### 2. Silver Layer — `02_silver_transformation.ipynb`
Cleans each table (removes duplicates, drops nulls in key fields, converts timestamps) and joins Customers → Orders → Order Items → Products → Payments into a single consolidated table.

**Table created:** `silver_order_details` — 106K+ clean, joined records

### 3. Gold Layer — `03_gold_analytics.ipynb`
Aggregates the Silver table into four business-ready KPI tables:

| Table | Metric |
|---|---|
| `gold_monthly_revenue` | Revenue trend over time (year-month) |
| `gold_top_categories` | Total orders & revenue by product category |
| `gold_payment_analysis` | Transaction count & revenue by payment type |
| `gold_city_analysis` | Order count & revenue by customer city |

---

## Results

- **Total records processed:** 100,000+ orders
- **Top revenue city:** São Paulo (~2.3M revenue)
- **Peak revenue month:** November 2017
- **Dominant payment method:** Credit card

### Screenshots

**Bronze Layer — Raw Ingestion**
![Bronze Layer](screenshots/bronze_layer.png)

**Silver Layer — Cleaned & Joined Data**
![Silver Layer](screenshots/silver_layer.png)

**Gold Layer — Monthly Revenue Trend**
![Gold Dashboard](screenshots/gold_dashboard.png)

---

## How to Run

1. Create an Azure Databricks workspace (or use Databricks Community Edition)
2. Upload the 5 CSVs to a Unity Catalog Volume (or DBFS)
3. Import the 3 notebooks from `notebooks/` into your workspace
4. Update the `RAW_PATH` variable in `01_data_ingestion` to match your upload location
5. Run notebooks in order: `01_data_ingestion` → `02_silver_transformation` → `03_gold_analytics`

---

## Key Skills Demonstrated

- Medallion Architecture design (Bronze/Silver/Gold)
- PySpark data cleaning, transformation, and joins
- Delta Lake table management on Unity Catalog
- Business KPI design and Spark SQL aggregation
- End-to-end data pipeline development on a cloud lakehouse platform

---


