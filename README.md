# SQL_retail_sales
This repository contains sql project on retail sales
Retail Sales Analysis SQL Project
Project Overview
Project Title: Retail Sales Analysis
Level: Beginner
Database: p1_retail_db

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

Objectives
Set up a retail sales database: Create and populate a retail sales database with the provided sales data.
Data Cleaning: Identify and remove any records with missing or null values.
Exploratory Data Analysis (EDA): Perform basic exploratory data analysis to understand the dataset.
Business Analysis: Use SQL to answer specific business questions and derive insights from the sales data.
Project Structure
1. Database Setup
Database Creation: The project starts by creating a database named p1_retail_db.
Table Creation: A table named retail_sales is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.
CREATE DATABASE p1_retail_db;

-- Creating table
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

-- Data Cleaning
select * from retail_sales
where transactions_id is null
	or sale_date is null
	or customer_id is null
	or gender is null
	or age is null
	or category is null
	or quantiy is null
	or price_per_unit is null
	or cogs is null
	or total_sale is null;

-- There are 3 records which have quantity, price_per_unit, cogs and total_sale as null.
-- 		As these columns are crucial for analysis and null values do not add to it.
-- 		So Deleting those records
-- There are 10 records where age is null
-- 		These records are valuable for sales analysis 
-- 		and although age is relevant for demographic analysis, null value can be ignored when such analysis is done.
DELETE from retail_sales
where quantiy is null
  and price_per_unit is null
  and cogs is null
  and total_sale is null;

-- Data Exploration
-- How many records?
select count(*)
from retail_sales;

-- How many unique customers do we have?
select count (distinct customer_id)
from retail_sales;

-- How many unique categories we have?
select count(distinct category)
from retail_sales;


-- Data Analysis and Business Problems.

-- Q.1 Write a SQL query to retrieve all columns for sales made on '2022-11-05'
select * from
retail_sales
where sale_date ='2022-11-05';

-- Q.2 Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 3 in the month of Nov-2022
select * from
retail_sales
where category = 'Clothing'
and to_char(sale_date,'YYYY-MM') ='2022-11'
and quantiy>3;

-- Q.3 Write a SQL query to calculate the total sales (total_sale) for each category.
select category,
sum(total_sale) as total_sales
from retail_sales
group by category;

-- Q.4 Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.
select round(avg(age),2) as average_age
from retail_sales
where category ='Beauty';

-- Q.5 Write a SQL query to find all transactions where the total_sale is greater than 1000.
select * from retail_sales
where total_sale>1000;

-- Q.6 Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.
select gender, category, count(transactions_id)
from retail_sales
group by gender, category;

-- Q.7 Write a SQL query to calculate the average sale for each month. Find out best selling month in each year
select * from
(select extract (year from sale_date) as year,
	 extract (month from sale_date) as month,
	 avg(total_sale) as average_sale,
	 rank() over (partition by extract(year from sale_date) order by avg(total_sale) desc) as rnk
	 from retail_sales
group by 1, 2 )
where rnk = 1;

-- Q.8 Write a SQL query to find the top 5 customers based on the highest total sales
select customer_id , sum(total_sale) as total_sales
from retail_sales
group by 1
order by 2 desc
limit 5;

-- Q.9 Write a SQL query to find the number of unique customers who purchased items from each category.
select count(distinct customer_id) as unique_customer_count, category
from retail_sales
group by 2;

-- Q.10 Write a SQL query to create each shift and number of orders Example Morning <=12, Afternoon Between 12 & 17, Evening >17)
with hourly_sale as
( select *, case when extract (hour from sale_time) <12 then 'Morning'
			when extract (hour from sale_time) between 12 and 17 then 'Afternoon'
			else 'Evening'
	   end as shift
	 from retail_sales
)
select shift , count(*) as total_orders
from hourly_sale
group by shift;
Findings
Customer Demographics: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
High-Value Transactions: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
Sales Trends: Monthly analysis shows variations in sales, helping identify peak seasons.
Customer Insights: The analysis identifies the top-spending customers and the most popular product categories.
Reports
Sales Summary: A detailed report summarizing total sales, customer demographics, and category performance.
Trend Analysis: Insights into sales trends across different months and shifts.
Customer Insights: Reports on top customers and unique customer counts per category.
Conclusion
This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.
