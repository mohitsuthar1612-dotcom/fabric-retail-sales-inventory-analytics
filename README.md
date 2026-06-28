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
* Fabric Data Pipeline that runs the PySpark ETL notebook manually and records run status
* Dataflow Gen2 ingestion from the World Bank public REST API into a Lakehouse country-reference table

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
## Multi-Source Integration

This project integrates two public data sources:

* **UCI Online Retail Excel dataset** for transaction-level sales, product, customer, and return analysis.
* **World Bank Country REST API** for country, region, and income-level reference data.

The country reference data is loaded through Dataflow Gen2 into `dbo.DimCountryReference`. A Fabric notebook creates `gold.DimCountry`, which connects to `gold.FactSales` through `CountryKey`.

This enables sales analysis by World Bank region and income level.

## Evidence

| Evidence                                     | Screenshot                                                     |
| -------------------------------------------- | -------------------------------------------------------------- |
| Lakehouse Bronze, Silver, and Gold layers    | `screenshots/01_lakehouse_medallion_layers.png`                |
| PySpark quality checks                       | `screenshots/02_notebook_data_quality_checks.png`              |
| Notebook pipeline run                        | `screenshots/03_pipeline_notebook_run_succeeded.png`           |
| API Dataflow output table                    | `screenshots/04_dataflow_api_country_reference_table.png`      |
| Final sequential API-to-notebook pipeline run| `screenshots/07_sequential_pipeline_api_then_notebook.png` |
| Country-to-sales semantic model relationship | `screenshots/06_semantic_model_country_sales_relationship.png` |



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

Microsoft Fabric, OneLake, Lakehouse, Medallion Architecture, PySpark, Delta Tables, Data Quality Checks, Star Schema Modelling, Direct Lake, Power BI, DAX, Fabric Data Pipelines, Notebook Activity, Dataflow Gen2, REST API Ingestion, Power Query.


## Next Improvements
* Connect country reference data to sales reporting through a country mapping rule
* Add a Copy Job or database source
* Add pipeline parameters, error handling, and refresh monitoring
* Document data lineage and business rules

