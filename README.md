# 🛍️ Retail Sales Analysis — SQL Project

## 📌 Project Overview

I worked on this project to analyze retail sales data using **SQL**. The analysis covers database setup, data cleaning, exploratory analysis, and business-focused questions around sales, customers, product categories, and transaction patterns.

---

## 🗄️ Database Setup

I created a database and a `retail_sales` table containing transaction, customer, product, and sales information.

```sql
CREATE DATABASE sql_project;

-- Table creation
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
```
---

## 🧹 Data Cleaning

I first checked the dataset for missing values across the important columns.

- Found **3 records** with missing `quantity`, `price_per_unit`, `cogs`, and `total_sale` values.
- Removed these records as the missing sales information made them unsuitable for sales analysis.
- Found **10 records** with missing `age` values.
- Retained these records since the remaining transaction information was still useful. The records can be excluded when performing age-based analysis.

```sql
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
```
---

## 🔎 Exploratory Data Analysis

I explored the dataset to understand:

- Total number of transactions
- Number of unique customers
- Number of product categories

```sql
-- How many records?
select count(*)
from retail_sales;

-- How many unique customers do we have?
select count (distinct customer_id)
from retail_sales;

-- How many unique categories we have?
select count(distinct category)
from retail_sales;
```
---

## 📊 Business Analysis

I used SQL to answer business questions related to:

- Sales made on a specific date
- Clothing purchases above a certain quantity
- Total sales by product category
- Average customer age for the Beauty category
- High-value transactions
- Transactions by gender and category
- Best-performing month in each year
- Top 5 customers by total spending
- Unique customers across categories
- Orders across Morning, Afternoon, and Evening shifts

```sql
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
```
---

## 💡 Key Insights

The analysis helped identify:

- **Category-level sales performance**
- **High-value transactions**
- **Top-spending customers**
- **Monthly sales patterns**
- **Customer distribution across categories**
- **Transaction patterns across different time shifts**

---

## 🛠️ Skills Used

**SQL | PostgreSQL | Data Cleaning | EDA | Aggregations | CTEs | Window Functions | CASE Statements | Date & Time Functions**


---
