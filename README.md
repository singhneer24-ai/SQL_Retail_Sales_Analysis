# Retail Sales Analysis SQL Project

## Project Overview

**Project Title**: Retail Sales Analysis  
**Level**: Beginner  
**Database**: `p1_retail_db`

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `p1_retail_db`.
 **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

 '''sql
 CREATE DATABASE Project1;

create table  retail_sales(transactions_id INT not null primary key,
	sale_date date not null,
	sale_time time not null,
	customer_id int not null,
	gender varchar(10) not null,
	age int not null,
	category varchar(15) not null,
	quantiy int not null,
	price_per_unit float not null,
	cogs float not null,
	total_sale float not null);
    
    SELECT * FROM retail_sales;
select COUNT(*) from retail_sales;
select quantiy from retail_sales
where quantiy = null;

--Q1 how many sales we have? 
select SUM(total_sale) as total_sales from retail_sales;

--Q2 how many unique customer we have?

select count(distinct customer_id) from retail_sales;

--Q3 how many category we have
select distinct category from retail_sales;

-- Business Key Problems and answers

--Q4 write a SQL query to retrive all columns for sale made on '2022-11-05'

SELECT * from retail_sales
where sale_date = '2022-11-05';

--Q5  write a SQL query to retrive all transactions where the category is 'Clothing'
-- and the quantity sold is more than 10 in the month of Nov-2022.

select *
from retail_sales
where category = 'Clothing'
and
date_format(sale_date, '%Y-%m') = '2023-11'
and quantiy >= 4;

--Q6 write a sql query for total sale of eaach category

select category, sum(total_sale) as net_sale,
count(*) as total_orders
from retail_sales
group by category;

--Q7 find average age of customer who purchase item from "Beauty" category

select round(avg(age),2) from retail_sales
where category = 'Beauty';

--Q8 write a sql query where total_sales is greater than 1000

select * from retail_sales
where total_sale > '1000';

-- Q9 Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category

SELECT 
    category,
    gender,
    COUNT(transactions_id) AS total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY category, gender;


-- 7.Write a SQL query to calculate the average sale for each month. Find out best selling month in each year

SELECT
    year,
    month,
    avg_sale
FROM (
    SELECT
        EXTRACT(YEAR FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        AVG(total_sale) AS avg_sale,
        RANK() OVER (
            PARTITION BY EXTRACT(YEAR FROM sale_date)
            ORDER BY AVG(total_sale) DESC)
            AS _rank
    FROM retail_sales
    GROUP BY year, month
) AS t1
WHERE _rank = 1;


-- 8.Write a SQL query to find the top 5 customers based on the highest total sales

select 
customer_id,
SUM(total_sale) as total_sales
From retail_sales 
group by customer_id
order by total_sales desc
LIMIT 5;

-- 9.Write a SQL query to find the number of unique customers who purchased items from each category

select 
category,
COUNT(distinct customer_id) as un_cst_id
from retail_sales
group by category;

-- 10.Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17):

WITH hourly_sale AS
(
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) AS total_orders    
FROM hourly_sale
GROUP BY shift;

-- ## Findings

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.

## Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales trends across different months and shifts.
- **Customer Insights**: Reports on top customers and unique customer counts per category.

## Conclusion

This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.

## How to Use

1. **Clone the Repository**: Clone this project repository from GitHub.
2. **Set Up the Database**: Run the SQL scripts provided in the `database_setup.sql` file to create and populate the database.
3. **Run the Queries**: Use the SQL queries provided in the `analysis_queries.sql` file to perform your analysis.
4. **Explore and Modify**: Feel free to modify the queries to explore different aspects of the dataset or answer additional business questions.
-- END OF PROJECT
'''
