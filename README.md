 Maven Market Sales Analysis - Power BI Dashboard
A comprehensive business intelligence dashboard analyzing sales performance, customer behavior, and product profitability for Maven Market — a multi-national grocery chain operating across the USA, Canada, and Mexico (1997-1998).

________________________________________
📸 Dashboard Preview

<img width="975" height="546" alt="image" src="https://github.com/user-attachments/assets/2e3a120b-6b2f-47aa-8c8f-5e30186ebd2c" />

 ________________________________________
 Project Overview
Detail	Info
Tool	Microsoft Power BI Desktop
Dataset	Maven Market (1997-1998)
Records	269,720 transactions
Countries	USA, Canada, Mexico
Stores	24 locations
Products	1,560 items across 20 brands
________________________________________
 Key Findings
•	 Total Revenue: $1,764,546 across 2 years
•	 Total Transactions: 269,720
•	 Top Performing Brand: Highest revenue and profit margin brand identified
•	 Return Rate: Monitored against 0.90% benchmark threshold
•	 Weekend Transactions: Tracked as % of total sales
•	 Revenue Target: 5% growth target set above current revenue
________________________________________ Data Model
This project uses a Star Schema with 2 fact tables and 5 dimension tables.
Tables
Table	Type	Records	Description
Transactions	Fact	269,720	All sales transactions
Returns_1997-1998	Fact	~5,000	Product return records
Customers	Dimension	10,000	Customer demographics
Products	Dimension	1,560	Product catalog & pricing
Stores	Dimension	24	Store locations
Regions	Dimension	5	Geographic regions
Calendar	Dimension	730 days	Date table (1997-1998)
Relationships
         Calendar
             ↓
       Transactions ←→ Customers
             ↓               
          Products ←→ Returns_1997-1998
             ↓
           Stores → Regions
________________________________________
 DAX Measures (24 Total)
Core Metrics
Measure	Description
Total revenue	SUMX of quantity × retail price
Total Cost	SUMX of quantity × product cost
Total Profit	Revenue minus cost per transaction
Profit Margin	Profit as % of revenue
Total Transactions	COUNT of all transactions
Quantity sold	SUM of all units sold
Time Intelligence
Measure	Description
Current Month Transactions	MTD transaction count
Current Month Profit	MTD profit
Current Month Returns	MTD returns
Last Month Transactions	Prior month comparison
Last Month Revenue	Prior month comparison
Last Month Profit	Prior month comparison
Last Month Returns	Prior month comparison
YTD revenue	Year-to-date revenue
60-Day Revenue	Rolling 60-day window
Returns & Performance
Measure	Description
Total Returns	COUNT of return transactions
Quantity returned	SUM of returned units
Return Rate	Returns divided by quantity sold
All returns	Returns ignoring filters
All transactions	Transactions ignoring filters
Transactions Analysis
Measure	Description
Weekend Transactions	Transactions on weekends only
% Weekend Transactions	Weekend share of total
Unique Products	Distinct product count
Revenue Target	Total revenue × 1.05
📄 See dax_formulas.md for full formula code.
________________________________________
 Dashboard Features
•	Interactive Bookmarks — Navigate between different report views using buttons
•	KPI Cards — Revenue, Transactions, Profit Margin, Return Rate with month-over-month comparison
•	Matrix Table — Product brand performance with conditional formatting
•	Map Visual — Store locations across USA, Canada, Mexico
•	Treemap — Revenue distribution by product category
•	Slicers — Filter by date, country, product category
________________________________________
 Repository Structure
Maven Market Report/
│
├── Maven Market Report.pbix     ← Power BI project file
├── README.md                   ← Project overview (this file)
├── screenshot.png              ← Dashboard preview image
└── docs/
     ├── data_dictionary.md          ← Column definitions for all tables
  ├── dax_formulas.md             ← All 24 DAX measure formulas
└── data/
    ├── MavenMarket_Customers.csv
    ├── MavenMarket_Products.csv
    ├── MavenMarket_Stores.csv
    ├── MavenMarket_Regions.csv
    ├── MavenMarket_Calendar.csv
    ├── MavenMarket_Transactions_1997.csv
    └── MavenMarket_Transactions_1998.csv
________________________________________
Tools & Skills Demonstrated
•	Microsoft Power BI Desktop — Data modeling, visualization, DAX
•	Power Query (M) — Data transformation and cleaning
•	DAX — 24 custom measures including time intelligence
•	Data Modeling — Star schema with relationships
•	Business Intelligence — KPIs, benchmarks, trend analysis
•	Data Visualization — Maps, matrices, treemaps, KPI cards
________________________________________
 How to Use
1.	Clone or download this repository
2.	Open Maven Market Report.pbix in Power BI Desktop
3.	If prompted about data sources, update file paths: 
o	Go to Home → Transform data
o	Update each table source to point to the /data folder
o	Click Close & Apply
4.	Explore the dashboard using the navigation buttons
________________________________________
 Documentation
File	Description
data_dictionary.md	All table and column definitions
dax_formulas.md	All 24 DAX measures with explanations
________________________________________
 About
Shoafzal
Aspiring Data Analyst
•	GitHub: https://github.com/ShoafzalDataAnalyst
•	LinkedIn: https://www.linkedin.com/in/shoafzal-shomuhidov-15b647389/en/
•	Email: shomuhidov.shoafzal@gmail.com
________________________________________
This project was completed as part of a data analytics portfolio. Dataset provided by Maven Analytics.

