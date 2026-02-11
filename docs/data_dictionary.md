 DAX Formulas - Maven Market Analysis
All custom measures created in the Maven Market Power BI project, organized by category.
________________________________________
 Summary
Category	Count
Core Sales Metrics	6
Profitability	3
Returns Analysis	5
Time Intelligence	7
Transactions Analysis	3
Total Measures	24
________________________________________
1) Core Sales Metrics
Total Revenue
Calculates total sales revenue by multiplying quantity sold by retail price for each transaction.
dax
Total revenue = SUMX(
    Transactions,
    Transactions[quantity] * RELATED(Products[product_retail_price])
)
Total Cost
Calculates total cost by multiplying quantity sold by product cost for each transaction.
dax
Total Cost = SUMX(
    Transactions,
    Transactions[quantity] * RELATED(Products[product_cost])
)
Total Transactions
Counts the total number of transactions recorded.
dax
Total Transactions = COUNT(Transactions[quantity])
Quantity Sold
Returns the total number of units sold across all transactions.
dax
Quantity sold = SUM(Transactions[quantity])
Unique Products
Counts the number of distinct products that appear in the product catalog.
dax
Unique Products = DISTINCTCOUNT(Products[product_name])
Revenue Target
Sets a 5% growth target above current total revenue. Used in KPI visuals.
dax
Revenue Target = [Total revenue] * 1.05
________________________________________
2) Profitability
Total Profit
Calculates profit for each transaction (retail price minus cost) then sums all results.
dax
Total Profit = SUMX(
    Transactions,
    Transactions[quantity] * (RELATED(Products[product_retail_price]) - RELATED(Products[product_cost]))
)
Profit Margin
Returns profit margin as a percentage of total revenue.
dax
Profit Margin = CALCULATE(
    ([Total revenue] - [Total Cost]) / [Total revenue] * 100
)
60-Day Revenue
Calculates rolling revenue for the last 60 days from the latest date in context.
dax
60-Day Revenue = 
CALCULATE(
    [Total Revenue],
    DATESINPERIOD(
        'Calendar'[Date],
        MAX('Calendar'[Date]),
        -60,
        DAY
    )
)
________________________________________
3) Returns Analysis
Total Returns
Counts the total number of return transactions recorded.
dax
Total Returns = COUNT('Returns_1997-1998'[quantity])
Quantity Returned
Sums the total number of units returned across all return transactions.
dax
Quantity returned = SUM('Returns_1997-1998'[quantity])
Return Rate
Calculates the ratio of total returns to total quantity sold. Used to monitor product quality.
dax
Return Rate = CALCULATE(
    DIVIDE(
        [Total Returns],
        [Quantity sold]
    )
)
! Benchmark: Return rate above 0.90% is flagged as underperforming.
All Returns
Counts total returns ignoring all active filters. Used for comparison in visuals.
dax
All returns = CALCULATE(
    COUNTROWS('Returns_1997-1998'),
    REMOVEFILTERS('Returns_1997-1998')
)
All Transactions
Counts total transactions ignoring all active filters. Used for comparison in visuals.
dax
All transactions = CALCULATE(
    COUNTROWS(Transactions),
    REMOVEFILTERS(Transactions)
)
________________________________________
4) Time Intelligence
Current Month Transactions
Returns the transaction count from the start of the current month to the latest date.
dax
Current Month Transactions = 
    CALCULATE([Total Transactions], DATESMTD('Calendar'[Date]))
Current Month Profit
Returns the total profit from the start of the current month to the latest date.
dax
Current Month Profit = 
    CALCULATE([Total Profit], DATESMTD('Calendar'[Date]))
Current Month Returns
Returns the total returns from the start of the current month to the latest date.
dax
Current Month Returns = 
    CALCULATE([Total Returns], DATESMTD('Calendar'[Date]))
Last Month Transactions
Returns total transactions from the same period one month prior. Used for month-over-month comparison.
dax
Last Month Transactions = 
CALCULATE(
    [Total Transactions],
    DATEADD('Calendar'[Date], -1, MONTH)
)
Last Month Revenue
Returns total revenue from the same period one month prior.
dax
Last Month Revenue = 
CALCULATE(
    [Total Revenue],
    DATEADD('Calendar'[Date], -1, MONTH)
)
Last Month Profit
Returns total profit from the same period one month prior.
dax
Last Month Profit = 
CALCULATE(
    [Total Profit],
    DATEADD('Calendar'[Date], -1, MONTH)
)
Last Month Returns
Returns total returns from the same period one month prior.
dax
Last Month Returns = 
CALCULATE(
    [Total Returns],
    DATEADD('Calendar'[Date], -1, MONTH)
)
YTD Revenue
Calculates cumulative revenue from the start of the year to the current date.
dax
YTD revenue = CALCULATE(
    [Total revenue],
    DATESYTD('Calendar'[date])
)
________________________________________
5) Transactions Analysis
Weekend Transactions
Counts transactions that occurred on weekends only. Requires a Weekend column in the Calendar table.
dax
Weekend Transactions = 
CALCULATE(
    COUNT(Transactions[quantity]),
    'Calendar'[Weekend] = "Y"
)
% Weekend Transactions
Returns the percentage of total transactions that occurred on weekends.
dax
% Weekend Transactions = 
    DIVIDE(
        [Weekend Transactions],
        [Total Transactions]
    )
________________________________________
🔗 Measure Dependencies
Some measures rely on other measures. Here is the dependency chain:
Total revenue ──────────────────────┐
Total Cost ─────────────────────────┼──→ Profit Margin
Total Profit ← (revenue - cost) ────┘
     │
     ├──→ Current Month Profit
     └──→ Last Month Profit

Total Transactions ─────────────────┐
     │                              ├──→ % Weekend Transactions
     └──→ Current Month Transactions│
     └──→ Last Month Transactions   │
                                    │
Weekend Transactions ───────────────┘

Total Returns ──────────────────────┐
Quantity Sold ──────────────────────┴──→ Return Rate
     │
     └──→ Current Month Returns
     └──→ Last Month Returns

Total revenue ──────────────────────→ Revenue Target (×1.05)
________________________________________
 DAX Functions Used
Function	Purpose	Used In
SUMX	Row-by-row calculation	Total Revenue, Total Cost, Total Profit
COUNT	Count rows	Total Transactions, Total Returns
DISTINCTCOUNT	Count unique values	Unique Products
SUM	Simple sum	Quantity Sold, Quantity Returned
CALCULATE	Modify filter context	Most measures
DIVIDE	Safe division (no errors)	Return Rate, % Weekend Transactions
RELATED	Lookup from related table	Revenue, Cost, Profit calculations
DATESINPERIOD	Rolling date window	60-Day Revenue
DATESMTD	Month-to-date filter	Current Month measures
DATEADD	Shift date by period	Last Month measures
DATESYTD	Year-to-date filter	YTD Revenue
REMOVEFILTERS	Remove all filters	All Returns, All Transactions
MAX	Latest date in context	60-Day Revenue
________________________________________
 Notes
•	Table names: Transactions, Returns_1997-1998, Products, Calendar
•	Date column: 'Calendar'[Date] used for all time intelligence functions
•	Weekend flag: 'Calendar'[Weekend] column must be set to "Y" or "N"
•	Profit Margin: Returns a value × 100 (e.g., 59.67 means 59.67%)
•	Return Rate: Returns a decimal (e.g., 0.009 means 0.9%) — format as percentage in visuals
________________________________________
Last Updated: February 2026
Author: Shoafzal
Tool: Microsoft Power BI Desktop

