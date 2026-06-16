## 📦 Data Catalog – Gold Layer

Once the data passes through Bronze and Silver, the Gold layer is the finish 
line. This is where raw, messy source data finally becomes something a business 
analyst can open and immediately make sense of. The model follows a Star Schema 
— two dimension tables describing *who* and *what*, and one fact table capturing 
*what happened*.

---

### 🧑‍💼 gold.dim_customers

Every customer record in this view is a result of merging two source systems — 
CRM and ERP — into a single, trustworthy profile. One thing worth noting: 
gender data comes from CRM as the preferred source, but where CRM falls short, 
ERP steps in as a backup. Small detail, but it matters when your analysis 
depends on it.

| Column Name | Data Type | Description |
|---|---|---|
| customer_key | INT | Surrogate key — system generated, used for joins |
| customer_id | INT | Original customer ID as it exists in CRM |
| customer_number | NVARCHAR(50) | Business-facing customer code |
| first_name | NVARCHAR(50) | Customer's first name |
| last_name | NVARCHAR(50) | Customer's last name |
| country | NVARCHAR(50) | Country pulled from ERP location records |
| marital_status | NVARCHAR(50) | Marital status from CRM |
| gender | NVARCHAR(50) | CRM-first, ERP as fallback, 'n/a' if neither available |
| birthdate | DATE | Date of birth from ERP |
| create_date | DATE | When the customer was first added to CRM |

---

### 📦 gold.dim_products

This view only includes products that are currently active — discontinued items 
are deliberately excluded by filtering out anything with an end date. The 
product details come from CRM, while category and subcategory information is 
enriched from the ERP side.

| Column Name | Data Type | Description |
|---|---|---|
| product_key | INT | Surrogate key — system generated, used for joins |
| product_id | INT | Original product ID from CRM |
| product_number | NVARCHAR(50) | Business-facing product code |
| product_name | NVARCHAR(50) | Full product name |
| category_id | NVARCHAR(50) | ID used to link to ERP category data |
| category | NVARCHAR(50) | Top-level product category from ERP |
| subcategory | NVARCHAR(50) | More granular product grouping from ERP |
| maintenance | NVARCHAR(50) | Maintenance type classification |
| cost | INT | Product cost at time of record |
| product_line | NVARCHAR(50) | Which product line this item belongs to |
| start_date | DATE | Date the product went live |

---

### 🛒 gold.fact_sales

This is the heart of the model. Each row here is one sales transaction — tied 
to a specific customer and product through surrogate keys. If you want to know 
what sold, to whom, when, and for how much — this is the table you query. Keep 
in mind this table references the dimension views, so always make sure those 
exist before querying here.

| Column Name | Data Type | Description |
|---|---|---|
| order_number | NVARCHAR(50) | Unique ID for each sales order |
| product_key | INT | Links to gold.dim_products |
| customer_key | INT | Links to gold.dim_customers |
| order_date | DATE | When the order was placed |
| shipping_date | DATE | When the order left the warehouse |
| due_date | DATE | Expected delivery date |
| sales_amount | INT | Total value of the order line |
| quantity | INT | Units ordered |
| price | INT | Price per unit at time of sale |
