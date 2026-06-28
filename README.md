# Retail Sales and Inventory Analytics Platform

A personal Microsoft Fabric learning project using real public retail transaction data.

## Project Goal

Build a simple end-to-end analytics solution in Microsoft Fabric to analyse retail sales, returns, products, customers, and estimated inventory status.

## Data Source

UCI Online Retail dataset: UK online retail transactions from December 2010 to December 2011.

## What I Built

* Fabric workspace and Lakehouse
* Bronze, Silver, and Gold data layers
* PySpark notebook for ingestion, transformation, and data-quality checks
* Delta tables in OneLake
* Star schema with sales, inventory, date, product, and customer tables
* Direct Lake Power BI semantic model
* DAX measures for sales, quantity sold, average order value, inventory value, and low-stock product count
* Initial Power BI dashboard with KPI cards

## Architecture

```text
Excel Source File
      |
      v
Bronze: Raw Lakehouse Table
      |
      v
Silver: Cleaned Retail Transactions
      |
      v
Gold: DimDate, DimProduct, DimCustomer,
      FactSales, FactInventory
      |
      v
Direct Lake Semantic Model
      |
      v
Power BI Report
```

## Important Data Limitations

* The source dataset contains transactions, not actual warehouse inventory.
* `FactInventory` is a simulated learning dataset based on product sales history.
* Product categories are derived from product-description keywords and are not official source categories.
* This is a portfolio learning project, not a production enterprise platform.

## Repository Structure

```text
docs/         Project documentation and screenshots
notebooks/    Exported Fabric notebook
screenshots/  Fabric, model, and report evidence
```

## Skills Demonstrated

Microsoft Fabric, OneLake, Lakehouse, Medallion Architecture, PySpark, Delta Tables, Data Quality Checks, Star Schema Modelling, Direct Lake, Power BI, DAX.

## Next Improvements

* Add Data Pipeline or Copy Job ingestion
* Add Dataflow Gen2 transformation example
* Add a second source such as CSV or REST API
* Add pipeline monitoring and refresh documentation
* Add more report pages and business questions
