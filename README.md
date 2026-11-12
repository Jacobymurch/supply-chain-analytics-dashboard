# Supply Chain & Inventory Performance Tracker

An interactive Power BI business intelligence dashboard designed to monitor multi-warehouse stock levels, track product category distribution, and flag low-stock SKUs requiring immediate reorder review[cite: 13, 14].

![Dashboard Preview](dashboard.png)

## Business Objective
Supply chain operations teams need real-time visibility into inventory distribution across regional hubs to prevent stockouts and overstocking[cite: 13]. This report consolidates raw inventory logs, product catalogs, and warehouse location data into an executive summary view[cite: 13, 14]:
* **Total Stock Monitored:** 475 units across 3 distribution hubs (East Hub, North Hub, South Hub)[cite: 13].
* **Actionable Alerts:** Automatically flags critical low-stock items (2 urgent items) for immediate procurement review[cite: 13].
* **Interactive Filtering:** Enables regional drill-downs across Beaumont, Dallas, and Houston[cite: 13].

## Data Architecture & Modeling
To ensure fast query performance and accurate cross-filtering, the raw flat exports were transformed and modeled into a normalized **Star Schema** following Microsoft PL-300 best practices:

![Dashboard Preview](star-schema.png)

* **Fact Table (inventory_log):** Stores transactional stock levels (StockLevel), TransactionID, Status, and conditional reorder flags (ReorderAlert)[cite: 14].
* **Dimension Tables (products & locations):** Connected to the central fact table via single-direction 1:* relationships using SKU and WarehouseID keys[cite: 14].
* **ETL / Power Query:** Cleaned raw CSV/Excel tables by promoting headers, enforcing data types, and engineering a conditional ReorderAlert column to isolate urgent inventory thresholds.

## Key DAX Measures
All KPI cards and dynamic breakdowns are powered by explicit DAX measures rather than implicit column aggregations:

```dax
Total Stock = SUM(inventory_log[StockLevel])
