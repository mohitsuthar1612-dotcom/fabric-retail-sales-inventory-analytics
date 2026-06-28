# Data Dictionary

## Source and Processing Tables

| Table                        | Layer     | Purpose                                                                                                    |
| ---------------------------- | --------- | ---------------------------------------------------------------------------------------------------------- |
| `bronze.online_retail_raw`   | Bronze    | Unchanged source transaction data loaded from the UCI Online Retail Excel file.                            |
| `silver.online_retail_clean` | Silver    | Cleaned retail transactions with duplicates removed, null handling, data types, and return classification. |
| `dbo.DimCountryReference`    | Reference | World Bank API country reference data loaded through Dataflow Gen2.                                        |

## Gold Reporting Tables

| Table                | Grain                                    | Purpose                                                                         |
| -------------------- | ---------------------------------------- | ------------------------------------------------------------------------------- |
| `gold.DimDate`       | One row per calendar date                | Supports reporting by day, month, quarter, and year.                            |
| `gold.DimProduct`    | One row per product stock code           | Stores product name, latest unit price, and keyword-derived product category.   |
| `gold.DimCustomer`   | One row per customer-country combination | Stores customer identifiers and country information.                            |
| `gold.DimCountry`    | One row per retail country label         | Connects retail countries to World Bank region and income-level reference data. |
| `gold.FactSales`     | One row per invoice line                 | Stores sales and return transactions.                                           |
| `gold.FactInventory` | One row per product snapshot             | Stores simulated inventory estimates for learning purposes.                     |

## Important Columns

### `gold.FactSales`

| Column            | Meaning                                                       |
| ----------------- | ------------------------------------------------------------- |
| `InvoiceNo`       | Retail invoice identifier.                                    |
| `DateKey`         | Date key in `YYYYMMDD` format used to join to `DimDate`.      |
| `ProductKey`      | Product stock code used to join to `DimProduct`.              |
| `CustomerKey`     | Customer identifier used to join to `DimCustomer`.            |
| `CountryKey`      | Standardized retail country key used to join to `DimCountry`. |
| `Quantity`        | Number of product units. Negative values represent returns.   |
| `UnitPrice`       | Price per unit.                                               |
| `SalesAmount`     | `Quantity × UnitPrice`; returns create negative values.       |
| `TransactionType` | `Sale` or `Return`.                                           |

### `gold.DimCountry`

| Column                 | Meaning                                                               |
| ---------------------- | --------------------------------------------------------------------- |
| `CountryKey`           | Standardized country join key.                                        |
| `RetailCountry`        | Country label found in the UCI retail source.                         |
| `ISO2Code`             | Two-letter country code from World Bank reference data.               |
| `Region`               | World Bank geographic region.                                         |
| `IncomeLevel`          | World Bank income classification.                                     |
| `ReferenceMatchStatus` | Indicates whether the retail label matched World Bank reference data. |

### `gold.FactInventory`

| Column                 | Meaning                                                      |
| ---------------------- | ------------------------------------------------------------ |
| `ProductKey`           | Product stock code.                                          |
| `SnapshotDate`         | Date the simulated inventory snapshot was generated.         |
| `EstimatedStockOnHand` | Estimated stock quantity based on historical sales volume.   |
| `InventoryValue`       | Estimated stock quantity multiplied by estimated unit price. |
| `InventoryStatus`      | Low Stock, Medium Stock, or Healthy Stock.                   |

## Business Rules and Limitations

* Negative quantities are retained and labelled as returns.
* Missing customer IDs are retained as `UNKNOWN`.
* Product categories are derived from product-description keywords.
* Inventory values are simulated from sales history and are not source-provided warehouse inventory.
* Country enrichment uses World Bank reference data; unmatched retail labels remain visible for data-quality transparency.
