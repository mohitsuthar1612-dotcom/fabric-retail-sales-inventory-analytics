# Architecture and Data Lineage

## Sources

1. **UCI Online Retail Excel dataset**
   Real public transaction data used for sales, product, customer, and return analysis.

2. **World Bank Country REST API**
   Public country reference data used for region and income-level enrichment.

## Data Flow

```text
UCI Excel File
   → Bronze: online_retail_raw
   → Silver: online_retail_clean
   → Gold: DimDate, DimProduct, DimCustomer, FactSales, FactInventory

World Bank REST API
   → Dataflow Gen2
   → dbo.DimCountryReference
   → Gold: DimCountry

Gold Tables
   → Direct Lake Semantic Model
   → Power BI Report
```

## Orchestration

`Retail_ETL_Pipeline` runs:

* `Refresh_Country_Reference_API`
* `Run_Retail_ETL_Notebook`

These activities currently run independently in parallel.

## Data Quality Rules

* Remove exact duplicate records
* Reject blank invoice numbers, stock codes, invalid dates, zero quantities, and negative prices
* Keep negative quantities as returns
* Keep missing customer IDs as `UNKNOWN`

## Important Limitations

* Inventory is simulated from sales history; it is not source-provided warehouse stock.
* Product categories are keyword-derived, not official source categories.
* This is a personal Fabric learning project, not a production system.
