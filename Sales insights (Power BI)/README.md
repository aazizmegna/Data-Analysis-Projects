## Sales Insights Data Analysis Project
In this project we are going to use a MYSQL sales database call db_dump.sql


STEPS: Sql database --> ETL --> Store to data warehouse --> Power BI dashboard

I. Problem Statement

II. Data Discovery

1. Setup mysql and Mysql workbench on your local machine

2.  Import the database into our Mysql Workbench application. 
In order to fully discover our data, we will write some queries such as:

-- Show all customer records

    `SELECT * FROM customers;`

-- Show total number of customers

    `SELECT count(*) FROM customers;`

-- Show transactions for Chennai market (market code for chennai is Mark001

    `SELECT * FROM transactions where market_code='Mark001';`

-- Show distrinct product codes that were sold in chennai

    `SELECT distinct product_code FROM transactions where market_code='Mark001';`

-- Show transactions where currency is US dollars

    `SELECT * from transactions where currency="USD"`

-- Show transactions in 2020 join by date table

    `SELECT transactions.*, date.* FROM transactions INNER JOIN date ON transactions.order_date=date.date where date.year=2020;`

-- Show total revenue in year 2020,

    `SELECT SUM(transactions.sales_amount) FROM transactions INNER JOIN date ON transactions.order_date=date.date where date.year=2020 and transactions.currency="INR\r" or transactions.currency="USD\r";`
	
-- Show total revenue in year 2020, January Month,

    `SELECT SUM(transactions.sales_amount) FROM transactions INNER JOIN date ON transactions.order_date=date.date where date.year=2020 and and date.month_name="January" and (transactions.currency="INR\r" or transactions.currency="USD\r");`

-- Show total revenue in year 2020 in Chennai

    `SELECT SUM(transactions.sales_amount) FROM transactions INNER JOIN date ON transactions.order_date=date.date where date.year=2020
and transactions.market_code="Mark001";`



Data Analysis Using Power BI
============================

III. Transform data (Power Query Editor)

-- Install Power BI
-- we will pull our data in Power BI then we will do ETL task and  build a data model which is gonna be suitable for doing our analysis
-- we will talk about Star Schema(google it)
-- In the markets table, we will remove New York an Paris (Just apply a filter)
-- In transactions table, we don't want value with -1 or 0
-- We want to convert sales_amount which are in USD in INR (to do that we will add a conditional column then modify the formula)
   
   `= Table.AddColumn(#"Filtered Rows", "norm_amount", each if [currency] = "USD" or [currency] ="USD#(cr)" then [sales_amount]*75 else [sales_amount], type any)`

VI. Build a dashboard

Click on "Enter data" and create "BaseMeasure" table. This table is the one we will use to plot different UI elements on our dashboard.

- Create a New Measure "Revenue" in BaseMeasure. this measure is the total of our sales_amountin transactions table

  Revenue = SUM('sales transactions'[sales_amount])

- Create a New Measure "Sales Qty" in BaseMeasure. this measure is the sum of our sales_qty in transactions table

  Sales Qty = SUM('sales transactions'[sales_qty])

Then we will plot these:

- Revenue by Customers
- Sales Quantity by Customers
- Revenue by Market
- Sales Quantity by Market
- Revenue by date

