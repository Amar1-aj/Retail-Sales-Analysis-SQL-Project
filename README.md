# Retail-Sales-Analysis-SQL-Project

/* This project focuses on performing data cleaning, exploration, and analysis on a retail sales dataset using SQL.
The objective is to extract meaningful insights from transactional data and answer key business questions. */

--📂 Project Structure
Database Creation:

CREATE DATABASE sql_project_retails;

--Table Creation:

CREATE TABLE retail_sales (...); (Schema to be added based on dataset structure)

--Data Import:

--Data is imported into the retail_sales table for analysis.

--🧹 Data Cleaning
/* Performed several checks to ensure data quality:

Checked for NULL values in each column

Deleted rows with any NULL values */

--Validated the structure using SELECT and COUNT queries

SELECT * FROM retail_sales
WHERE transactions_id IS NULL 
   OR sale_date IS NULL 
   OR sale_time IS NULL 
   OR customer_id IS NULL 
   OR gender IS NULL 
   OR age IS NULL 
   OR category IS NULL 
   OR quantiy IS NULL 
   OR price_per_unit IS NULL 
   OR cogs IS NULL 
   OR total_sale IS NULL;

  -- 📊 Data Exploration
/* Key metrics extracted:

Total Sales Records

Unique Customers

Sales Categories */

SELECT COUNT(*) AS total_sales FROM retail_sales;
SELECT COUNT(DISTINCT customer_id) AS unique_customers FROM retail_sales;
SELECT DISTINCT category FROM retail_sales;


--📈 Business Questions Answered

--Q1: Sales on Specific Date

SELECT * FROM retail_sales WHERE sale_date = '2022-11-05';

--Q2: High Quantity Clothing Sales in Nov-2022

SELECT * FROM retail_sales
WHERE category = 'Clothing'
  AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
  AND quantiy >= 4;

--Q3: Total Sales by Category

SELECT category, SUM(total_sale) AS net_sale FROM retail_sales GROUP BY category;

--Q4: Average Age of Beauty Category Customers

SELECT ROUND(AVG(age), 2) FROM retail_sales WHERE category = 'Beauty';

--Q5: High-Value Transactions

SELECT * FROM retail_sales WHERE total_sale > 1000;

--Q6: Transactions by Gender & Category

SELECT category, gender, COUNT(*) FROM retail_sales GROUP BY category, gender;

--Q7: Best-Selling Month Each Year

SELECT year, month, avg_sale FROM (
  SELECT 
    EXTRACT(YEAR FROM sale_date) AS year,
    EXTRACT(MONTH FROM sale_date) AS month,
    AVG(total_sale) AS avg_sale,
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) AS rank
  FROM retail_sales
  GROUP BY 1,2
) AS ranked_months
WHERE rank = 1;

--Q8: Top 5 Customers by Total Sales

SELECT customer_id, SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;

--Q9: Unique Customers per Category

SELECT category, COUNT(DISTINCT customer_id)
FROM retail_sales
GROUP BY category;

--Q10: Orders by Time Shift

WITH hourly_sale AS (
  SELECT *, 
    CASE
      WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
      WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
      ELSE 'Evening'
    END AS shift
  FROM retail_sales
)
SELECT shift, COUNT(*) AS total_orders FROM hourly_sale GROUP BY shift;

/* 🧠 Key Findings
Identified the top-performing product categories

Found customer purchase behavior patterns across gender and time

Determined peak sales months

Highlighted top customers contributing highest revenue */

/* 🛠️ Tools Used
PostgreSQL (or SQL-compliant database)

SQL for data cleaning, transformation, and querying */

/* 📌 Notes
Schema definition is assumed during table creation and should be explicitly defined.

Ensure to update table structure in the SQL file if uploading the full project. */








