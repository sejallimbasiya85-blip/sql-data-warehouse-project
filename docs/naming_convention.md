# Naming Conventions

This document outlines the naming standards followed across all layers of this
data warehouse project — covering schemas, tables, views, columns, and stored
procedures. Keeping these consistent makes the project easier to navigate,
maintain, and hand off.

---

## Table of Contents
1. [General Principles](#general-principles)
2. [Table Naming Conventions](#table-naming-conventions)
   - [Bronze Layer](#bronze-layer)
   - [Silver Layer](#silver-layer)
   - [Gold Layer](#gold-layer)
3. [Column Naming Conventions](#column-naming-conventions)
   - [Surrogate Keys](#surrogate-keys)
   - [Technical Columns](#technical-columns)
4. [Stored Procedures](#stored-procedures)

---

## General Principles

- **Format**: snake_case throughout — lowercase letters, words separated by underscores (`_`)
- **Language**: English only for all object names
- **Reserved Words**: Never use SQL reserved words as object names

---

## Table Naming Conventions

### Bronze Layer

Tables in the Bronze layer stay as close to the source as possible — no renaming, no restructuring.

**Pattern**: `<sourcesystem>_<entity>`

| Part | Description |
|---|---|
| `<sourcesystem>` | Name of the source system e.g. `crm`, `erp` |
| `<entity>` | Original table name from the source system |

**Example**: `crm_cust_info` → Customer info table sourced from the CRM system

---

### Silver Layer

Same naming pattern as Bronze — source system prefix is retained, table names stay consistent with their origin.

**Pattern**: `<sourcesystem>_<entity>`

| Part | Description |
|---|---|
| `<sourcesystem>` | Name of the source system e.g. `crm`, `erp` |
| `<entity>` | Original table name from the source system |

**Example**: `erp_loc_a101` → Location data sourced from the ERP system

---

### Gold Layer

Tables here use business-friendly names rather than source system names. A category prefix describes the role of each table.

**Pattern**: `<category>_<entity>`

| Part | Description |
|---|---|
| `<category>` | Role of the table e.g. `dim`, `fact`, `report` |
| `<entity>` | Business-aligned descriptive name |

**Examples**:
- `dim_customers` → Dimension table holding customer attributes
- `fact_sales` → Fact table holding sales transactions

#### Category Prefix Glossary

| Prefix | Role | Examples |
|---|---|---|
| `dim_` | Dimension table | `dim_customers`, `dim_products` |
| `fact_` | Fact table | `fact_sales` |
| `report_` | Reporting table | `report_sales_monthly` |

---

## Column Naming Conventions

### Surrogate Keys

Every dimension table has a surrogate primary key. These always end with `_key`
so they're immediately recognizable in any query or join.

**Pattern**: `<tablename>_key`

**Example**: `customer_key` → Surrogate key in `dim_customers`

---

### Technical Columns

System-generated metadata columns are prefixed with `dwh_` to distinguish them
from business data columns at a glance.

**Pattern**: `dwh_<columnname>`

**Example**: `dwh_load_date` → Tracks when the record was loaded into the warehouse

---

## Stored Procedures

All stored procedures responsible for loading data into a layer follow a simple,
consistent pattern so it's always clear what each procedure does and where it loads.

**Pattern**: `load_<layer>`

| Procedure | Purpose |
|---|---|
| `load_bronze` | Loads raw data from source CSV files into the Bronze layer |
| `load_silver` | Transforms and loads cleaned data into the Silver layer |
| `load_gold` | Builds the final Gold layer views for reporting |
