🛍️ Retail Sales Analysis — SQL Project
📌 Project Overview

Project Title: Retail Sales Analysis
Level: Beginner
Database: p1_retail_db

This project demonstrates fundamental SQL skills and techniques used by data analysts to explore, clean, and analyze retail sales data.

The project covers the complete analysis workflow, including:

🗄️ Database and table creation
🧹 Data cleaning and handling missing values
🔎 Exploratory Data Analysis (EDA)
📊 Business-oriented sales analysis
👥 Customer and demographic analysis
📈 Sales trend analysis

The goal is to use SQL to answer practical business questions and extract meaningful insights from retail transaction data.

🎯 Objectives
1. Database Setup

Create a retail sales database and populate it with the provided sales data.

2. Data Cleaning

Identify records containing missing or NULL values and determine how they should be handled.

3. Exploratory Data Analysis

Explore the dataset to understand its size, customers, categories, and overall structure.

4. Business Analysis

Answer business questions using SQL queries and identify useful patterns in sales and customer behavior.

🗂️ Project Structure
1. Database Setup
Create Database
CREATE DATABASE p1_retail_db;
Create Table
CREATE TABLE retail_sales
(
    transactions_id int primary key,
    sale_date date,
    sale_time time,
    customer_id int,
    gender varchar(15),
    age int,
    category varchar(15),
    quantiy int,
    price_per_unit float,
    cogs float,
    total_sale float
);
🧹 2. Data Cleaning

The first step was to identify records containing missing values.

SELECT *
FROM retail_sales
WHERE transactions_id IS NULL
   OR sale_date IS NULL
   OR customer_id IS NULL
   OR gender IS NULL
   OR age IS NULL
   OR category IS NULL
   OR quantiy IS NULL
   OR price_per_unit IS NULL
   OR cogs IS NULL
   OR total_sale IS NULL;
Missing Data Observations
3 records had NULL values in quantity, price_per_unit, cogs, and total_sale.
These columns are essential for sales analysis, so these records were removed.
10 records had NULL values in age.
These records were retained because the remaining transaction information was still useful for sales analysis. The missing age values can simply be excluded when performing demographic analysis.
Remove incomplete sales records
DELETE FROM retail_sales
WHERE quantiy IS NULL
  AND price_per_unit IS NULL
  AND cogs IS NULL
  AND total_sale IS NULL;
🔎 3. Exploratory Data Analysis (EDA)
Number of Records
SELECT COUNT(*)
FROM retail_sales;
Number of Unique Customers
SELECT COUNT(DISTINCT customer_id)
FROM retail_sales;
Number of Unique Product Categories
SELECT COUNT(DISTINCT category)
FROM retail_sales;
📊 4. Data Analysis & Business Problems
Q1. Retrieve all sales made on November 5, 2022
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
Q2. Find Clothing transactions with more than 3 items sold in November 2022
SELECT *
FROM retail_sales
WHERE category = 'Clothing'
  AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
  AND quantiy > 3;
Q3. Calculate total sales for each category
SELECT 
    category,
    SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY category;
Q4. Find the average age of customers purchasing from the Beauty category
SELECT 
    ROUND(AVG(age), 2) AS average_age
FROM retail_sales
WHERE category = 'Beauty';
Q5. Find transactions where total sales exceeded 1,000
SELECT *
FROM retail_sales
WHERE total_sale > 1000;
Q6. Find the number of transactions by gender and category
SELECT 
    gender,
    category,
    COUNT(transactions_id) AS total_transactions
FROM retail_sales
GROUP BY gender, category;
Q7. Find the best-selling month based on average sales for each year
SELECT *
FROM
(
    SELECT 
        EXTRACT(YEAR FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        AVG(total_sale) AS average_sale,
        RANK() OVER (
            PARTITION BY EXTRACT(YEAR FROM sale_date)
            ORDER BY AVG(total_sale) DESC
        ) AS rnk
    FROM retail_sales
    GROUP BY 1, 2
)
WHERE rnk = 1;
Q8. Find the Top 5 Customers based on total sales
SELECT 
    customer_id,
    SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
Q9. Find the number of unique customers for each category
SELECT 
    category,
    COUNT(DISTINCT customer_id) AS unique_customer_count
FROM retail_sales
GROUP BY category;
Q10. Analyze sales by time-based shifts

The transactions were categorized into three shifts:

🌅 Morning: Before 12 PM
☀️ Afternoon: 12 PM–5 PM
🌙 Evening: After 5 PM
WITH hourly_sale AS
(
    SELECT *,
        CASE 
            WHEN EXTRACT(HOUR FROM sale_time) < 12 
                THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 
                THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM retail_sales
)

SELECT 
    shift,
    COUNT(*) AS total_orders
FROM hourly_sale
GROUP BY shift;
💡 Key Findings
👥 Customer Demographics

The dataset contains customers across different age groups, with purchases distributed across multiple product categories, including Clothing and Beauty.

💰 High-Value Transactions

Several transactions exceeded 1,000 in total sales, indicating the presence of higher-value purchases within the dataset.

📈 Sales Trends

Monthly analysis reveals variations in average sales across different months and years, helping identify periods with stronger sales performance.

🛍️ Customer Insights

The analysis identifies the top-spending customers and measures the number of unique customers purchasing from each product category.

⏰ Sales by Shift

Transactions were analyzed across Morning, Afternoon, and Evening shifts to understand when customer orders are most frequent.

📑 Reports

The analysis can be summarized into three key reporting areas:

Sales Summary
Total sales
Category performance
Transaction volume
Customer demographics
Trend Analysis
Monthly sales trends
Peak sales periods
Sales by time-based shift
Customer Insights
Top-spending customers
Unique customers by category
Gender-based transaction analysis
🏁 Conclusion

This project provides a practical introduction to SQL for data analysis, covering the complete workflow from database creation and data cleaning to exploratory analysis and business-driven queries.

Through this project, SQL was used to analyze:

📊 Sales performance
👥 Customer behavior
🛍️ Product categories
📈 Sales trends
⏰ Transaction timing
💰 High-value purchases

The analysis demonstrates how SQL can transform raw retail transaction data into structured insights that can support business decision-making.

🛠️ Skills Demonstrated
SQL
PostgreSQL
Data Cleaning
Exploratory Data Analysis (EDA)
Aggregations
GROUP BY
CASE WHEN
Date & Time Functions
Common Table Expressions (CTEs)
Window Functions
Ranking
Customer Analysis
Sales Analysis
📁 Project Structure
SQL_retail_sales/
│
├── retail_sales.sql
└── README.md
👩‍💻 Author

Yamini Thakur

Aspiring Data Analyst | SQL | Excel | Python | Power BI | Tableau
