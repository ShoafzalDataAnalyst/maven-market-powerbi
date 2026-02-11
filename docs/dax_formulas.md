 Data Dictionary - Maven Market Analysis
Overview
This document provides detailed information about the data structure used in the Maven Market Power BI analysis project. The dataset contains sales transaction data for a multi-national grocery chain operating across the USA, Canada, and Mexico from 1997-1998.
________________________________________
 Tables Overview
Table	Records	Purpose	Key Field
Customers	10,000	Customer demographic information	customer_id
Products	1,560	Product catalog with pricing	product_id
Stores	24	Store location and details	store_id
Regions	5	Geographic regions	region_id
Calendar	730 days	Date dimension table	date
Transactions	269,720	Sales transactions (1997-1998)	transaction_date, customer_id, product_id, store_id
Returns_1997-1998	~5,000	Product returns	return_date, product_id, store_id
________________________________________
 Table Details
1) Customers Table
Contains demographic information about Maven Market customers.
Column Name	Data Type	Description	Example Values	Notes
customer_id	Integer	Unique customer identifier	1, 2, 3...	Primary Key
customer_acct_num	Text	Customer account number	C1001, C1002	Alternative ID
customer_first_name	Text	Customer's first name	John, Maria, Wei	-
customer_last_name	Text	Customer's last name	Smith, Garcia, Chen	-
customer_birthdate	Date	Date of birth	1985-03-15	For age calculations
customer_marital_status	Text	Marital status	S (Single), M (Married)	Single character code
customer_gender	Text	Gender	M (Male), F (Female)	Single character code
customer_total_children	Integer	Number of children	0, 1, 2, 3...	0 = no children
customer_num_children_at_home	Integer	Children living at home	0, 1, 2, 3...	≤ total_children
customer_education	Text	Education level	High School, Bachelors, Graduate Degree	Categorical
customer_member_card	Text	Loyalty card type	Normal, Silver, Golden	Membership tier
customer_occupation	Text	Job type	Professional, Management, Skilled Manual	-
customer_homeowner	Text	Home ownership status	Y (Yes), N (No)	Y/N flag
customer_annual_income	Integer	Yearly income (USD)	30000, 50000, 100000	In dollars
customer_address	Text	Street address	123 Main St	-
customer_city	Text	City name	Portland, Vancouver, Guadalajara	-
customer_state_province	Text	State/Province	OR, BC, Jalisco	2-letter code or full name
customer_postal_code	Text	ZIP/Postal code	97201, V5K 0A1	Format varies by country
customer_country	Text	Country	USA, Canada, Mexico	Full name
Relationships:
•	Links to Transactions via customer_id
________________________________________
2) Products Table
Product catalog with cost and pricing information.
Column Name	Data Type	Description	Example Values	Notes
product_id	Integer	Unique product identifier	1, 2, 3...	Primary Key
product_sku	Text	Stock Keeping Unit code	SKU-1001	Internal code
product_name	Text	Full product name	Whole Milk 1 Gallon, Organic Apples	Descriptive name
product_brand	Text	Brand name	Hermanos, Washington, Tell Tale	20 unique brands
product_category	Text	Product category	Dairy, Produce, Snacks	High-level grouping
product_subcategory	Text	Detailed category	Milk, Fresh Fruit, Chips	More specific
product_weight	Decimal	Weight in pounds	1.5, 2.0, 0.5	Pounds (lbs)
product_retail_price	Decimal	Selling price (USD)	3.99, 5.49, 1.29	Customer pays this
product_cost	Decimal	Cost to store (USD)	2.50, 3.20, 0.89	What store pays
product_recyclable	Text	Recyclable packaging	Y (Yes), N (No)	Environmental flag
product_low_fat	Text	Low-fat product	Y (Yes), N (No)	Health flag
Calculated Fields (DAX):
•	Profit per Unit = product_retail_price - product_cost
•	Profit Margin % = (Profit / product_retail_price) × 100
Relationships:
•	Links to Transactions via product_id
•	Links to Returns_1997-1998 via product_id
________________________________________
3) Stores Table
Store location and operational information.
Column Name	Data Type	Description	Example Values	Notes
store_id	Integer	Unique store identifier	1, 2, 3...	Primary Key
store_number	Integer	Store number	1, 2, 3..24	24 stores total
store_name	Text	Store location name	Portland Store, Vancouver Store	City-based naming
store_type	Text	Store format	Supermarket, Gourmet Supermarket	2 types
store_phone	Text	Contact number	(503) 555-0123	Phone format
store_address	Text	Street address	456 Oak Avenue	-
store_city	Text	City name	Portland, Acapulco, Merida	-
store_state	Text	State/Province	OR, WA, BC, Jalisco	-
store_country	Text	Country	USA, Canada, Mexico	3 countries
store_postal_code	Text	ZIP/Postal code	97201	-
store_square_feet	Integer	Store size	35000, 42000, 28000	Floor space in sq ft
store_manager	Text	Manager name	John Anderson	Full name
region_id	Integer	Region identifier	1, 2, 3, 4, 5	Foreign Key to Regions
Relationships:
•	Links to Transactions via store_id
•	Links to Returns_1997-1998 via store_id
•	Links to Regions via region_id
________________________________________
4) Regions Table
Geographic region information.
Column Name	Data Type	Description	Example Values	Notes
region_id	Integer	Unique region identifier	1, 2, 3, 4, 5	Primary Key
sales_district	Text	District name	District 1, District 2	Sales territory
sales_region	Text	Region name	West, North, Central	Geographic grouping
Relationships:
•	Links to Stores via region_id
________________________________________
5) Calendar Table
Date dimension for time-based analysis.
Column Name	Data Type	Description	Example Values	Notes
date	Date	Calendar date	1997-01-01, 1998-12-31	Primary Key
day	Integer	Day of month	1, 2, 3...31	-
month	Integer	Month number	1, 2, 3...12	1=January
month_name	Text	Month name	January, February	Full name
quarter	Integer	Quarter	1, 2, 3, 4	Q1=Jan-Mar
year	Integer	Year	1997, 1998	2 years of data
week	Integer	Week number	1, 2, 3...52	ISO week
weekday	Integer	Day of week number	1=Monday, 7=Sunday	1-7
weekday_name	Text	Day name	Monday, Tuesday	Full name
start_of_week	Date	Week start date	1997-01-06 (Monday)	Always Monday
start_of_month	Date	Month start date	1997-01-01	First day of month
start_of_quarter	Date	Quarter start date	1997-01-01	First day of quarter
start_of_year	Date	Year start date	1997-01-01	January 1st
Relationships:
•	Links to Transactions via transaction_date = date
•	Links to Returns_1997-1998 via return_date = date
________________________________________
6) Transactions Table
Fact table containing all sales transactions.
Column Name	Data Type	Description	Example Values	Notes
transaction_date	Date	Date of purchase	1997-01-01 to 1998-12-31	Foreign Key to Calendar
transaction_id	Integer	Unique transaction ID	100001, 100002	Primary Key (composite)
customer_id	Integer	Customer who purchased	1, 2, 3...	Foreign Key to Customers
product_id	Integer	Product purchased	1, 2, 3...	Foreign Key to Products
store_id	Integer	Store where purchased	1, 2, 3...24	Foreign Key to Stores
quantity	Integer	Units purchased	1, 2, 3, 5, 10	Number of units
stock_date	Date	Inventory stock date	1997-01-01	When stocked
Calculated Measures (DAX):
•	Total Transactions = COUNT(Transactions[quantity]) → 269,720
•	Total Revenue = SUMX(Transactions, Transactions[quantity] * RELATED(Products[product_retail_price]))
•	Total Cost = SUMX(Transactions, Transactions[quantity] * RELATED(Products[product_cost]))
•	Total Profit = SUMX(Transactions, Transactions[quantity] * (RELATED(Products[product_retail_price]) - RELATED(Products[product_cost])))
Relationships:
•	Many-to-One with Customers (customer_id)
•	Many-to-One with Products (product_id)
•	Many-to-One with Stores (store_id)
•	Many-to-One with Calendar (transaction_date)
________________________________________
7) Returns_1997-1998 Table
Product returns tracking.
Column Name	Data Type	Description	Example Values	Notes
return_date	Date	Date of return	1997-01-05 to 1998-12-30	Foreign Key to Calendar
product_id	Integer	Product returned	1, 2, 3...	Foreign Key to Products
store_id	Integer	Store where returned	1, 2, 3...24	Foreign Key to Stores
quantity	Integer	Units returned	1, 2, 3	Number of units
Calculated Measures (DAX):
•	Total Returns = COUNT('Returns_1997-1998'[quantity])
•	Quantity Returned = SUM('Returns_1997-1998'[quantity])
•	Return Rate = DIVIDE([Total Returns], [Quantity sold])
•	Return Rate Benchmark = 0.90% (target threshold)
Relationships:
•	Many-to-One with Products (product_id)
•	Many-to-One with Stores (store_id)
•	Many-to-One with Calendar (return_date)
________________________________________
 Data Model Relationships
Star Schema Structure
              Calendar (Dimension)
                  ↓
            Transactions (Fact)
          ↙      ↓      ↓      ↘
    Customers Products Stores  Returns_1997-1998
    (Dimension) (Dimension) (Dimension)  (Fact)
                                ↓
                            Regions
                          (Dimension)
Relationship Details
From Table	From Column	To Table	To Column	Cardinality	Cross-Filter
Transactions	customer_id	Customers	customer_id	Many-to-One	Single
Transactions	product_id	Products	product_id	Many-to-One	Single
Transactions	store_id	Stores	store_id	Many-to-One	Single
Transactions	transaction_date	Calendar	date	Many-to-One	Single
Returns_1997-1998	product_id	Products	product_id	Many-to-One	Single
Returns_1997-1998	store_id	Stores	store_id	Many-to-One	Single
Returns_1997-1998	return_date	Calendar	date	Many-to-One	Single
Stores	region_id	Regions	region_id	Many-to-One	Single
________________________________________
 Key Metrics & KPIs
Revenue Metrics
•	Total Revenue: $1,764,546.00 (1997-1998)
•	Average Transaction Value: ~$6.54
•	Revenue by Country: USA, Canada, Mexico breakdown
Operational Metrics
•	Total Transactions: 269,720
•	Total Customers: 10,000
•	Total Stores: 24 (across 3 countries)
•	Total Products: 1,560 (20 brands)
Performance Indicators
•	Profit Margin: Varies by product (calculated from retail price - cost)
•	Return Rate: ~0.9% (benchmark threshold)
•	Product Performance: Top brands by revenue, profit margin
Geographic Analysis
•	5 Regions: Regional sales performance
•	3 Countries: USA, Canada, Mexico
•	Top Performing Stores: Based on revenue and profitability
________________________________________
 Data Quality Notes
Date Range
•	Start Date: January 1, 1997
•	End Date: December 31, 1998
•	Duration: 2 complete years (730 days)
Data Completeness
•	 No missing customer records
•	 Complete product catalog
•	 All stores have region assignments
•	 Continuous date range in Calendar table
Business Rules
1.	Return Rate Threshold: Products with return rate > 0.90% are flagged
2.	Membership Tiers: Normal < Silver < Golden (customer segmentation)
3.	Profit Margin: All products must have positive margin
4.	Geographic Coverage: 3 countries, 5 regions, 24 stores
________________________________________
 Usage in Power BI Dashboard
Bookmarks Navigation
The dashboard includes interactive bookmarks for:
1.	Overview - High-level KPIs and metrics
2.	Product Analysis - Product performance by brand
3.	Geographic Insights - Map and regional breakdown
4.	Returns Analysis - Return rate monitoring
Filters & Slicers
•	Date Range: Select specific periods
•	Country: USA, Canada, Mexico
•	Product Category: Filter by product type
•	Store: Individual store performance
Visual Elements
•	KPI Cards: Revenue, Transactions, Profit Margin, Return Rate
•	Matrix Table: Product brand performance with conditional formatting
•	Map Visual: Store locations with revenue bubbles
•	Treemap: Product category revenue distribution
•	Column Chart: Revenue by month/quarter
________________________________________
💡 Tips for Data Analysis
Common Calculations
dax
// Revenue per Transaction
Revenue per Transaction = DIVIDE([Total revenue], [Total Transactions], 0)

// Customer Lifetime Value
CLV = DIVIDE([Total revenue], DISTINCTCOUNT(Transactions[customer_id]), 0)

// Stores with High Returns
High Return Stores = CALCULATE(
    COUNTROWS(Stores),
    FILTER(Stores, [Return Rate] > 0.009)
)
Useful Filters
•	High-value customers: Income > $70,000
•	Premium products: Profit Margin > 30%
•	Underperforming stores: Return Rate > benchmark
•	Seasonal trends: Compare Q1 vs Q4
________________________________________
 Support
For questions about this data dictionary or the Maven Market project:
•	GitHub: https://github.com/ShoafzalDataAnalyst
•	Email: shomuhidov.shoafzal@gmail.com
•	LinkedIn: https://www.linkedin.com/in/shoafzal-shomuhidov-15b647389/?locale=en
________________________________________
Last Updated: February 2026
Author: Shoafzal]

