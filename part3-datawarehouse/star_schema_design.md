Star Schema Design – Part 3 (Analytics \& Reporting)
📌 Overview

This document describes the Star Schema design used in Part 3 of the project for analytical and reporting purposes.

The star schema is built on top of cleaned transactional data from Part 1 (MySQL) and enriched with product insights from Part 2 (MongoDB).

The design enables:

Fast analytical queries

Simplified joins

Business-friendly reporting

⭐ Why Star Schema?

Optimized for read-heavy analytics

Simple structure with fact and dimension tables

Easy to use in BI tools (Power BI, Tableau)

Improves query performance compared to normalized schemas

🏗️ Schema Overview
dim\_customer
|
|
dim\_date —— fact\_sales —— dim\_product
|
|
dim\_status

🧮 Fact Table
1️⃣ fact\_sales
📄 Description

Stores transaction-level measurable metrics.

🧱 Structure
Column Name	Data Type	Description
sales\_key	INT	Surrogate primary key
transaction\_id	VARCHAR(10)	Source transaction ID
customer\_key	INT	FK to dim\_customer
product\_key	INT	FK to dim\_product
date\_key	INT	FK to dim\_date
status\_key	INT	FK to dim\_status
quantity	INT	Quantity sold
unit\_price	DECIMAL(10,2)	Price per unit
total\_amount	DECIMAL(12,2)	quantity × unit\_price
📦 Dimension Tables
2️⃣ dim\_customer
📄 Description

Stores customer descriptive attributes.

Column Name	Data Type	Description
customer\_key	INT	Surrogate primary key
customer\_id	VARCHAR(10)	Business key
first\_name	VARCHAR(50)	Customer first name
last\_name	VARCHAR(50)	Customer last name
city	VARCHAR(50)	Customer city
registration\_date	DATE	Registration date
3️⃣ dim\_product
📄 Description

Stores product attributes enriched with catalog data.

Column Name	Data Type	Description
product\_key	INT	Surrogate primary key
product\_id	VARCHAR(10)	Business key
product\_name	VARCHAR(100)	Product name
category	VARCHAR(30)	Product category
subcategory	VARCHAR(50)	Product subcategory
price	DECIMAL(10,2)	Product price
warranty\_months	INT	Warranty period

Data can be enriched from MongoDB catalog where applicable.

4️⃣ dim\_date
📄 Description

Stores date attributes for time-based analysis.

Column Name	Data Type	Description
date\_key	INT	YYYYMMDD
full\_date	DATE	Calendar date
day	INT	Day of month
month	INT	Month number
month\_name	VARCHAR(15)	Month name
quarter	INT	Quarter
year	INT	Year
weekday	VARCHAR(10)	Weekday name
5️⃣ dim\_status
📄 Description

Stores order status values.

Column Name	Data Type	Description
status\_key	INT	Surrogate primary key
status	VARCHAR(20)	Order status (Completed, Pending, Cancelled)
🔗 Relationships

fact\_sales.customer\_key → dim\_customer.customer\_key

fact\_sales.product\_key → dim\_product.product\_key

fact\_sales.date\_key → dim\_date.date\_key

fact\_sales.status\_key → dim\_status.status\_key

📈 Example Analytical Queries
Total Revenue by Category
SELECT
p.category,
SUM(f.total\_amount) AS revenue
FROM fact\_sales f
JOIN dim\_product p ON f.product\_key = p.product\_key
GROUP BY p.category;

Monthly Sales Trend
SELECT
d.year,
d.month\_name,
SUM(f.total\_amount) AS monthly\_revenue
FROM fact\_sales f
JOIN dim\_date d ON f.date\_key = d.date\_key
GROUP BY d.year, d.month\_name
ORDER BY d.year, d.month;

🧠 Design Considerations

Surrogate keys used for performance

Slowly Changing Dimensions (Type 1) assumed

Fact table contains only numeric measures

Dimensions contain descriptive attributes

🔮 Future Enhancements

Implement SCD Type 2 for customers and products

Add dim\_city or dim\_location

Add promotional or campaign dimension

Build materialized views for BI tools

✅ Part 3 Deliverables

Star schema design

Analytical SQL queries

Business-ready reporting model

👤 Author

Tushar Singh
