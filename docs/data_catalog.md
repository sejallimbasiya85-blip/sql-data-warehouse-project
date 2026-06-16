## 📦 Data Catalog – Gold Layer

The Gold layer contains business-ready views modeled as a **Star Schema**, consisting of two dimension tables and one fact table.

---

### 🧑‍💼 gold.dim_customers

**Description**: Stores cleaned and enriched customer information, integrating data from CRM and ERP source systems.

| Column Name | Data Type | Description |
|---|---|---|
| customer_key | INT | Surrogate primary key (auto-generated) |
| customer_id | INT | Original customer ID from CRM system |
| customer_number | NVARCHAR(50) | Unique customer identifier/code |
| first_name | NVARCHAR(50) | Customer's first name |
| last_name | NVARCHAR(50) | Customer's last name |
| country | NVARCHAR(50) | Country of residence (from ERP location data) |
| marital_status | NVARCHAR(50) | Customer's marital status |
| gender | NVARCHAR(50) | Customer's gender (CRM as primary, ERP as fallback) |
| birthdate | DATE | Customer's date of birth (from ERP) |
| create_date | DATE | Date the customer record was created in CRM |

---

### 📦 gold.dim_products

**Description**: Stores cleaned and enriched product information, integrating CRM product data with ERP category details. Excludes historical/discontinued products.

| Column Name | Data Type | Description |
|---|---|---|
| product_key | INT | Surrogate primary key (auto-generated) |
| product_id | INT | Original product ID from CRM system |
| product_number | NVARCHAR(50) | Unique product identifier/code |
| product_name | NVARCHAR(50) | Name of the product |
| category_id | NVARCHAR(50) | Category identifier (from CRM) |
| category | NVARCHAR(50) | High-level product category (from ERP) |
| subcategory | NVARCHAR(50) | Product subcategory (from ERP) |
| maintenance | NVARCHAR(50) | Maintenance classification of the product |
| cost | INT | Product cost |
| product_line | NVARCHAR(50) | Product line classification |
| start_date | DATE | Date the product became active |

---

### 🛒 gold.fact_sales

**Description**: Stores transactional sales data, linking to dimension tables for customers and products to enable analytical reporting.

| Column Name | Data Type | Description |
|---|---|---|
| order_number | NVARCHAR(50) | Unique sales order identifier |
| product_key | INT | Foreign key referencing gold.dim_products |
| customer_key | INT | Foreign key referencing gold.dim_customers |
| order_date | DATE | Date the order was placed |
| shipping_date | DATE | Date the order was shipped |
| due_date | DATE | Expected delivery/due date |
| sales_amount | INT | Total sales amount for the order line |
| quantity | INT | Quantity of product ordered |
| price | INT | Unit price of the product |
