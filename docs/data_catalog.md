## 📦 Data Catalog – Gold Layer

The Gold layer is where everything comes together. After all the cleaning and 
standardization done in the Silver layer, the Gold layer transforms that data 
into something analysts can actually use — a clean Star Schema with two 
dimension tables and one fact table.

---

### 🧑‍💼 gold.dim_customers

This view brings together customer data from both the CRM and ERP systems into 
one clean, reliable customer profile. Gender is sourced from CRM as the primary 
system, with ERP data used as a fallback wherever CRM has gaps.

| Column Name | Data Type | Description |
|---|---|---|
| customer_key | INT | Surrogate primary key, auto-generated for each customer |
| customer_id | INT | Original customer ID from the CRM system |
| customer_number | NVARCHAR(50) | Unique customer code used across systems |
| first_name | NVARCHAR(50) | Customer's first name |
| last_name | NVARCHAR(50) | Customer's last name |
| country | NVARCHAR(50) | Country of residence, sourced from ERP location data |
| marital_status | NVARCHAR(50) | Customer's marital status |
| gender | NVARCHAR(50) | Gender — CRM is primary source, ERP used as fallback |
| birthdate | DATE | Date of birth, sourced from ERP |
| create_date | DATE | Date the customer record was first created in CRM |

---

### 📦 gold.dim_products

A clean product reference table built from CRM product records enriched with 
ERP category information. Only active products are included here — anything 
with an end date (i.e. discontinued products) is filtered out intentionally.

| Column Name | Data Type | Description |
|---|---|---|
| product_key | INT | Surrogate primary key, auto-generated for each product |
| product_id | INT | Original product ID from the CRM system |
| product_number | NVARCHAR(50) | Unique product code used across systems |
| product_name | NVARCHAR(50) | Full name of the product |
| category_id | NVARCHAR(50) | Category identifier linked to ERP category data |
| category | NVARCHAR(50) | High-level product category from ERP |
| subcategory | NVARCHAR(50) | More specific product grouping from ERP |
| maintenance | NVARCHAR(50) | Maintenance classification of the product |
| cost | INT | Cost of the product |
| product_line | NVARCHAR(50) | Product line the item belongs to |
| start_date | DATE | Date the product became active |

---

### 🛒 gold.fact_sales

The core of the star schema — every sales transaction lives here. Each row 
represents a single order line and connects back to both the customer and 
product dimensions through surrogate keys, making it straightforward to 
slice and dice sales data any way you need.

| Column Name | Data Type | Description |
|---|---|---|
| order_number | NVARCHAR(50) | Unique identifier for each sales order |
| product_key | INT | Foreign key linking to gold.dim_products |
| customer_key | INT | Foreign key linking to gold.dim_customers |
| order_date | DATE | Date the order was placed |
| shipping_date | DATE | Date the order was shipped out |
| due_date | DATE | Expected delivery date for the order |
| sales_amount | INT | Total sales value for the order line |
| quantity | INT | Number of units ordered |
| price | INT | Unit price at the time of the order |
