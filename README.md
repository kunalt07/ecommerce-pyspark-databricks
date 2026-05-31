# 🛒 Brazilian E-Commerce Analytics Pipeline

An end-to-end data engineering project built on **Databricks** and **PySpark**, implementing a **Medallion Architecture** (Bronze → Silver → Gold) on real-world e-commerce data from Olist (Brazil), with analytics dashboards in **Tableau Public**.

---

## 📊 Live Dashboard

🔗 [View on Tableau Public](https://public.tableau.com/views/BrazilianE-CommerceAnalytics/E-CommerceAnalyticsStory)

---

## 🏗️ Architecture
Raw CSVs (9 files, 100K+ records)
↓
[Bronze Layer]
Delta Tables — Raw ingestion with audit columns
↓
[Silver Layer]
Cleaned, joined, enriched data
↓
[Gold Layer]
Business aggregations + Databricks SQL Views
↓
Tableau Public Dashboards

---

## 🖼️ Pipeline Screenshots

**Medallion Layer Tables in Databricks Catalog**
![Catalog](screenshots/catalog_tables.png)

**Bronze Ingestion Summary**
![Bronze](screenshots/bronze_summary.png)

**Silver Layer Complete**
![Silver](screenshots/silver_complete.png)

**Gold Layer Complete**
![Gold](screenshots/gold_complete.png)

---

## 📁 Project Structure
ecommerce-pyspark-databricks/
├── 01_bronze_ingestion.py       # Raw CSV → Delta Lake
├── 02_silver_cleaning.py        # Cleaning, joining, enrichment
├── 03_gold_aggregations.py      # Business metrics
├── 04_databricks_sql_views.py   # SQL views on Gold tables
├── 05_export_for_tableau.py     # CSV exports for Tableau
└── README.md

---

## 🔧 Tech Stack

| Layer | Tool |
|---|---|
| Processing | PySpark (Databricks) |
| Storage | Delta Lake (Unity Catalog Volumes) |
| Orchestration | Databricks Notebooks |
| SQL Layer | Databricks SQL Views |
| Visualization | Tableau Public |
| Source Data | Kaggle — Olist Brazilian E-Commerce |

---

## 📦 Dataset

- **Source:** [Olist Brazilian E-Commerce — Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
- **Size:** 100K+ orders, 9 relational tables
- **Period:** 2016 – 2018

**Tables ingested:**
- `orders`, `order_items`, `payments`, `reviews`
- `customers`, `sellers`, `products`
- `geolocation`, `category_name_translation`

---

## 🥉 Bronze Layer

- Ingested all 9 CSVs into Delta tables
- Added audit columns: `_ingested_at`, `_source_file`
- Ran data quality checks: null PKs, duplicate counts
- Registered tables in `ecommerce_bronze` database

---

## 🥈 Silver Layer

- Parsed and standardized all timestamps
- Derived `delivery_delay_days` and `is_late_delivery` flags
- Deduplicated reviews (kept latest per order)
- Aggregated payments to one row per order
- Joined products with English category names
- Averaged geolocation per zip code
- Built a master joined table across all entities

---

## 🥇 Gold Layer

5 aggregation tables built for analytics:

| Table | Description |
|---|---|
| `seller_performance` | Revenue, avg review, late delivery % per seller |
| `monthly_revenue` | Revenue trend by category and month |
| `state_distribution` | Customer count and revenue by Brazilian state |
| `late_delivery_heatmap` | Late delivery % by seller city |
| `category_performance` | Revenue and review score by product category |

---

## 📈 Databricks SQL Views

5 views built on Gold tables with additional business logic:

- `vw_seller_performance` — Seller tier classification (Top / Average / Underperformer)
- `vw_monthly_revenue_trend` — Cumulative revenue window function
- `vw_state_distribution` — Revenue share % per state
- `vw_late_delivery_heatmap` — Delivery risk level classification
- `vw_category_performance` — Revenue rank and share per category

---

## 📊 Dashboards (Tableau Public)

**Dashboard 1 — Revenue Overview**
- Monthly revenue trend by top 10 categories
- Top 10 categories by total revenue with avg review score

**Dashboard 2 — Seller Performance**
- Seller count by tier (Top / Average / Underperformer)
- Revenue vs review score scatter plot per seller

**Dashboard 3 — Delivery & Geography**
- Customer revenue choropleth map by Brazilian state
- Late delivery risk by seller city (top 20)

---

## 💡 Key Insights

- **Health & Beauty** is the highest revenue category at $1.23M
- **São Paulo** accounts for ~37% of total platform revenue
- Only **18.6%** of sellers qualify as Top Sellers
- **Tres de Maio** has the highest late delivery rate among cities

---

## ▶️ How to Run

1. Download dataset from Kaggle and upload CSVs to a Databricks Volume
2. Run notebooks in order: `01` → `02` → `03` → `04` → `05`
3. Each notebook builds on the previous layer
4. Query Gold tables via Databricks SQL or export CSVs for Tableau
