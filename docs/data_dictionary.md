# 📖 Data Dictionary — Chocolate Sales Dataset

> **Project:** SectionB\_G-17 · Chocolate Sales Performance Analysis
> **Institute:** Newton School of Technology · DVA Capstone 2
> **Maintained by:** Data Lead
> **Last updated:** April 2026

---

## How To Use This File

- Each section covers one source CSV file from `data/raw/`.
- Every column used in analysis, KPI computation, or Tableau filtering is documented below.
- **Cleaning Notes** capture every transformation applied in `02_Cleaning.ipynb`.
- **Used In** maps the column to the notebook(s) or Tableau view where it appears.
- Derived / computed columns are listed in their own section at the bottom.

---

## 📋 Dataset Summary

| Item | Details |
|------|---------|
| **Dataset name** | Chocolate Sales Dataset |
| **Source** | Internal Retail Simulation — Google Drive |
| **Raw file names** | `calender.csv`, `customers.csv`, `products.csv`, `sales.csv`, `stores.csv` |
| **Download links** | See `data/raw/raw.md` |
| **Last updated** | April 2026 |
| **Granularity** | One row per sales transaction in `sales.csv`; one row per entity in all dimension tables |
| **Schema type** | Star schema — `sales.csv` is the fact table; all others are dimension tables |

---

## 📁 File 1 — `sales.csv` *(Fact Table)*

> **Granularity:** One row per sales transaction.
> This is the central fact table. All revenue and volume KPIs are computed from this file.

| Column Name | Data Type | Description | Example Value | Used In | Cleaning Notes |
|-------------|-----------|-------------|---------------|---------|----------------|
| `SaleID` | string / int | Unique identifier for each sales transaction | `S-10042` | EDA / KPI / Tableau | Checked for duplicates; duplicates dropped. Assert uniqueness after cleaning. |
| `Date` | date | Date the transaction was recorded | `2023-03-15` | EDA / KPI / Tableau | Parsed to `datetime64` using `pd.to_datetime()`. Nulls flagged and dropped. |
| `CustomerID` | string | Foreign key linking to `customers.csv` | `C-2041` | EDA / KPI | Validated against customer dimension; unmatched IDs logged and investigated. |
| `ProductID` | string | Foreign key linking to `products.csv` | `P-105` | EDA / KPI / Tableau | Validated against product dimension; nulls dropped. |
| `StoreID` | string | Foreign key linking to `stores.csv` | `ST-07` | EDA / KPI / Tableau | Validated against store dimension; unmatched IDs investigated. |
| `Boxes` | int | Number of boxes sold in this transaction | `24` | EDA / KPI | Nulls filled with `0` where confirmed missing shipment. Negative values flagged as returns. |
| `Amount` | float | Total revenue for this transaction (currency units) | `4800.00` | EDA / KPI / Tableau | Nulls dropped. Checked for `Amount < 0` (returns) — handled separately. Cross-validated: `Amount ≈ Boxes × PricePerBox`. |

---

## 📁 File 2 — `products.csv` *(Product Dimension)*

> **Granularity:** One row per product SKU.

| Column Name | Data Type | Description | Example Value | Used In | Cleaning Notes |
|-------------|-----------|-------------|---------------|---------|----------------|
| `ProductID` | string | Unique product identifier; primary key | `P-105` | EDA / KPI / Tableau | Verified uniqueness. Used as join key with `sales.csv`. |
| `Product` | string | Full product name | `70% Dark Bites 500g` | EDA / Tableau | Stripped leading/trailing whitespace. Title-cased for consistency. |
| `Category` | string | Product category (e.g., Dark, Milk, White, Gifting) | `Dark` | EDA / KPI / Tableau | Standardised to title case. Null rows dropped. Used as primary dashboard filter. |
| `CostPerBox` | float | Unit cost to produce / procure one box (in currency units) | `120.00` | KPI | Nulls investigated; rows with missing cost excluded from margin calculation. |
| `PricePerBox` | float | Retail selling price per box (in currency units) | `200.00` | KPI / Tableau | Verified `PricePerBox ≥ CostPerBox` for all active SKUs. Exceptions flagged. |

---

## 📁 File 3 — `customers.csv` *(Customer Dimension)*

> **Granularity:** One row per unique customer.

| Column Name | Data Type | Description | Example Value | Used In | Cleaning Notes |
|-------------|-----------|-------------|---------------|---------|----------------|
| `CustomerID` | string | Unique customer identifier; primary key | `C-2041` | EDA / KPI / Tableau | Verified uniqueness. Join key with `sales.csv`. |
| `Customer` | string | Full customer name | `Priya Sharma` | EDA / Tableau | Stripped whitespace. Not used in aggregation — retained for Tableau tooltips only. |
| `Gender` | string | Customer gender | `Female` | EDA / Tableau | Standardised to `Male` / `Female` / `Other`. Null values categorised as `Unknown`. |
| `Age` | int | Customer age in years at time of dataset creation | `29` | EDA / KPI / Tableau | Outliers (Age < 10 or Age > 90) flagged and reviewed. Nulls dropped. Age bands derived (see Derived Columns). |

---

## 📁 File 4 — `stores.csv` *(Store Dimension)*

> **Granularity:** One row per store location.

| Column Name | Data Type | Description | Example Value | Used In | Cleaning Notes |
|-------------|-----------|-------------|---------------|---------|----------------|
| `StoreID` | string | Unique store identifier; primary key | `ST-07` | EDA / KPI / Tableau | Verified uniqueness. Join key with `sales.csv`. |
| `Store` | string | Store name or location label | `Mumbai Central` | Tableau | Stripped whitespace. Used for Tableau tooltip labels. |
| `Region` | string | Broad geographic region | `West` | EDA / KPI / Tableau | Standardised to title case. Null values flagged. Key dashboard filter. |
| `Country` | string | Country where the store operates | `India` | EDA / Tableau | Standardised to consistent country names. Used for geographic mapping in Tableau. |

---

## 📁 File 5 — `calender.csv` *(Date Dimension)*

> **Granularity:** One row per calendar date.
> Note: "calender" matches the raw file name exactly, although the standard spelling is "calendar".

| Column Name | Data Type | Description | Example Value | Used In | Cleaning Notes |
|-------------|-----------|-------------|---------------|---------|----------------|
| `Date` | date | Calendar date; primary key of the date dimension | `2023-03-15` | EDA / KPI / Tableau | Parsed to `datetime64`. Verified continuous date range with no gaps. |
| `Month` | string / int | Month number or abbreviated month name | `3` or `Mar` | EDA / KPI / Tableau | Standardised to integer month number for sorting; month name retained as label. |
| `Year` | int | Calendar year | `2023` | EDA / KPI / Tableau | Verified range. Used for YoY comparisons. |
| `Quarter` | string | Quarter label | `Q1` | EDA / KPI / Tableau | Derived from `Month` if not present. Standardised to `Q1`–`Q4` format. |
| `Weekday` | string | Day-of-week name | `Wednesday` | EDA | Standardised to full weekday name. Used for day-of-week sales pattern analysis. |
| `IsWeekend` | boolean / int | Flag indicating weekend (Saturday or Sunday) | `0` or `1` | EDA | Derived from `Weekday` if not present. `1` = weekend, `0` = weekday. |

---

## 🔧 Derived Columns

> These columns do not exist in the raw CSVs. They are computed during the cleaning and analysis pipeline.

| Derived Column | Source File(s) | Logic | Business Meaning |
|---------------|----------------|-------|-----------------|
| `GrossMargin` | `products.csv` | `PricePerBox − CostPerBox` | Absolute profit contribution per box sold |
| `GrossMarginPct` | `products.csv` | `(PricePerBox − CostPerBox) / PricePerBox × 100` | Percentage margin per SKU; used to identify loss-making products |
| `RevenuePerBox` | `sales.csv` | `Amount / Boxes` | Realised selling price per unit; cross-validates against `PricePerBox` |
| `AgeBand` | `customers.csv` | Bins: `<18`, `18–24`, `25–34`, `35–44`, `45–54`, `55+` on `Age` | Enables customer-segment dashboards without exposing raw age |
| `MonthYear` | `calender.csv` + `sales.csv` | `YYYY-MM` string derived from `Date` | Used for MoM trend charts in Tableau |
| `TotalTransactionCost` | `sales.csv` + `products.csv` | `Boxes × CostPerBox` | Total COGS per transaction; feeds margin KPI |
| `IsRepeatCustomer` | `sales.csv` | `1` if `CustomerID` appears in ≥ 2 `SaleID` rows, else `0` | Flags returning customers for loyalty and retention analysis |
| `StoreRevenueRank` | `sales.csv` + `stores.csv` | Dense rank of stores by `SUM(Amount)` | Normalised store performance benchmark |

---

## ⚠️ Data Quality Notes

1. **File naming inconsistency** — The raw file is named `calender.csv` (missing an 'a'). The filename is preserved exactly as-is in `data/raw/` but referenced correctly in all notebooks.

2. **Missing transactions** — A small number of `SaleID` rows were found with null `Amount` or `Boxes`. These have been investigated and dropped from the analytical dataset. See `02_Cleaning.ipynb` for row counts before and after.

3. **Currency not labelled** — The raw dataset does not specify the currency unit for `Amount`, `CostPerBox`, and `PricePerBox`. All monetary figures are treated as a single consistent currency throughout. Do not mix with external currency data without normalisation.

4. **Negative `Amount` values** — Some transactions contain negative `Amount` values, likely representing returns or refunds. These have been separated into a `returns` subset and excluded from revenue KPIs unless explicitly stated.

5. **Date range gaps** — The `calender.csv` date dimension should be verified for continuity. Any gaps in the calendar dimension may cause silent exclusion of transactions when performing date-dimension joins in Tableau.

6. **Customer–Product join completeness** — Not all `CustomerID` values in `sales.csv` have a matching record in `customers.csv`. Unmatched records represent ~X % of transactions and have been excluded from customer-segment analysis.

7. **Duplicate product names** — Multiple `ProductID` values may share the same `Product` name (e.g., same product in different sizes). Always join on `ProductID`, not on `Product` name.

8. **Outlier transactions** — Transactions with `Boxes > 500` in a single line item have been flagged as potential bulk/wholesale orders and may skew retail-focused KPIs. Flagged in `03_eda.ipynb`.

---

## 🔗 Join Map (Star Schema)

```
calender.csv (Date)
        │
        │ Date
        ▼
sales.csv ◄──────── customers.csv (CustomerID)
(Fact Table)
        │
        ├── ProductID ──► products.csv
        │
        └── StoreID ────► stores.csv
```

All joins are **left joins** from `sales.csv` to preserve the full transaction record, with unmatched dimension keys logged and handled as described in the Data Quality Notes above.

---

*For questions about this data dictionary, contact the Data Lead or raise a GitHub Issue in this repository.*
