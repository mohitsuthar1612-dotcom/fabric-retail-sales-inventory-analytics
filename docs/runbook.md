# Runbook

## Purpose

This document explains how to run the Retail Sales and Inventory Analytics Platform in Microsoft Fabric.

## Prerequisites

* Microsoft Fabric trial or Fabric-enabled workspace
* Access to the `Retail Sales and Inventory Analytics` workspace
* Access to `RetailLakehouse`
* UCI Online Retail Excel source file uploaded to:

```text
Files/bronze/Online Retail.xlsx
```

## Fabric Items

| Item                          | Purpose                                                                                                              |
| ----------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| `Country_Reference_Dataflow`  | Loads World Bank country reference data from a public REST API.                                                      |
| `Retail_ETL_Notebook`         | Loads retail data, performs data-quality checks, creates Bronze, Silver, and Gold tables, and enriches country data. |
| `Retail_ETL_Pipeline`         | Orchestrates the Dataflow and notebook in sequence.                                                                  |
| `Retail Sales Semantic Model` | Direct Lake semantic model used for reporting.                                                                       |

## Execution Order

Run:

```text
Retail_ETL_Pipeline
```

The pipeline runs these activities in order:

1. `Refresh_Country_Reference_API`
2. `Run_Retail_ETL_Notebook`

The notebook starts only after the country-reference Dataflow succeeds.

## Expected Lakehouse Outputs

```text
bronze.online_retail_raw
silver.online_retail_clean

gold.DimDate
gold.DimProduct
gold.DimCustomer
gold.DimCountry
gold.FactSales
gold.FactInventory

dbo.DimCountryReference
```

## Validation Checks

After a successful pipeline run, validate:

* Both pipeline activities show `Succeeded`
* `gold.DimCountry` exists and contains World Bank region and income-level fields
* `gold.FactSales` contains `CountryKey`
* Dimension keys have no duplicates
* Fact tables have no orphan records against their related dimensions
* The semantic model relationship exists between `DimCountry[CountryKey]` and `FactSales[CountryKey]`

## Important Limitations

* `FactInventory` is a simulated inventory snapshot based on sales history. It is not real stock-on-hand data.
* Product categories are derived from description keywords and are not source-provided categories.
* Country matching uses source-label standardisation rules. Some labels may remain unmatched.
* This project is a personal learning portfolio project, not a production deployment.
