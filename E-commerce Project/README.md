Ecommerce Analytics Project (Databricks)
========================================

This project demonstrates an end-to-end ETL pipeline in Databricks for an ecommerce dataset, transforming raw data into actionable insights for business decisions. The pipeline follows the **Bronze → Silver → Gold** architecture and uses **Delta tables** for reliable and efficient analytics.

Dataset
-------

The dataset contains detailed information about:

*   **Products**
    
*   **Customers**
    
*   **Orders**
    
*   **Reviews**
    

**Scope:** This data will be used to build dashboards and analytics reports that help managers and sales teams make informed business decisions.

Data Modeling
-------------

We use a **Star Schema** for the analytics layer.

*   **Reasoning:** Star schema is a BI best practice—it simplifies queries, improves performance, and is easy for analysts to use.
    

**Fact Table:** order\_items**Dimension Tables:** orders, customers, products, categories, reviews

**Key Relationships:**

*   order\_items.order\_id → orders.order\_id
    
*   orders.customer\_id → customers.customer\_id
    
*   order\_items.product\_id → products.product\_id
    
*   products.category\_id → categories.category\_id
    
*   review.customer\_id → customers.customer\_id
    

Bronze Layer
------------

**Purpose:** Store raw data in a structured format while performing basic quality checks.

**Steps:**

1.  Load raw CSV files into Spark DataFrames with schema inference and headers.
    
2.  Write raw data into the raw\_ecommerce database as Delta tables (landing layer).
    
3.  Perform **data quality checks**: check for NULLs in primary key columns to ensure integrity.
    
4.  Write curated Delta tables into the bronze\_ecommerce database.
    

**Reasoning:** The Bronze layer preserves raw data and ensures integrity, providing a reliable base for downstream processing.

Silver Layer
------------

**Purpose:** Clean and deduplicate Bronze data for analytics-ready use.

**Steps:**

1.  Load data from Bronze tables (order\_items, categories, customers).
    
2.  Inspect schema and data types.
    
3.  Identify and remove **duplicates** and check for **null values**.
    
4.  Persist cleaned tables into the silver\_ecommerce database in Delta format.
    

**Reasoning:** Cleaning and deduplication ensures accuracy and consistency, which is crucial for reliable analytics and reporting.

Gold Layer
----------

**Purpose:** Build analytics tables for business insights.

**Steps:**

1.  Load Silver tables: order\_items, orders, customers, products, categories, reviews.
    
2.  Create **sales\_performance\_gold**: joins orders, products, and categories; extracts year/month for time analysis.
    
3.  Create **customer\_insights\_gold**: aggregates total spend, last order date, and total orders per customer.
    
4.  Create **product\_reviews\_gold**: joins reviews with products/customers, adds sentiment analysis based on ratings.
    
5.  Save all Gold tables in Unity Catalog for downstream analytics.
    

**Business Questions Answered:**

*   Peak vs slow sales periods, top-performing products/categories, and overall sales trends.
    
*   Top spenders, loyal customers, high-value segments, and churn risk.
    
*   Product review sentiment enriched with customer and product context.
    

**Reasoning:** The Gold layer provides ready-to-use insights for managers and sales teams to make informed business decisions.