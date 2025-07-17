# Retail Sales Analysis Report

## Project Overview
I have performed an analysis of retail sales data using SQL. In which a few business questions have been addressed through SQL queries.

## Dataset used
-<a href=https://github.com/Kanissma/Retail-sales-analysis-report/blob/main/Retail%20Sales%20Analysis_dataset.csv>Dataset</a>

## Objective
1.	Setting up a database: Create a database and table for the data set available. 
2.	Data Cleaning: Check for the missing value and take the required actions to rectify it.
3.	Data exploration: Perform basic analysis to under the dataset available.
4.	Business analysis: Write SQL queries to answer business questions and derive insights through them.

## Process

## Setting up a database:
1. For this project a database is created named project_1 and a table named retail_sales is created under itwith required colums and the it's data type. Table consists of 11 columns.
Columns present in the table: transactions ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.
2. Perform a check on the data set available for the data type and import the data to the created table.

```sql
--Create database
CREATE DATABASE project_1;

--Create table
CREATE TABLE retail_sales(
transactions_id	INT PRIMARY KEY,
sale_date DATE,
sale_time TIME,
customer_id	INT,
gender varchar(20),
age	INT,
category varchar(50),
quantity INT,
price_per_unit float,	
cogs float,
total_sale float
);
```

## Data cleaning:
1. Perform check to find any null values available and deleted the missing records if the count is very low.

```sql
--To check null value records in table
SELECT * FROM retail_sales
WHERE transactions_id IS NULL
OR sale_date IS NULL 
OR sale_time IS NULL
OR customer_id IS NULL	
OR gender IS NULL	
OR age IS NULL	
OR category	IS NULL
OR quantity IS NULL	
OR price_per_unit IS NULL
OR cogs IS NULL
OR total_sale IS NULL;

--Data cleaning

DELETE FROM retail_sales
WHERE transactions_id IS NULL
OR sale_date IS NULL 
OR sale_time IS NULL
OR customer_id IS NULL	
OR gender IS NULL	
OR age IS NULL	
OR category	IS NULL
OR quantity IS NULL	
OR price_per_unit IS NULL
OR cogs IS NULL
OR total_sale IS NULL;
```

## Data exploration

1. View all the records available in the table and perform basic queries such as to count the total number of records available, total number of customers available, categories available for purchase.

```sql
--Q1 Total number of sales we have?
SELECT COUNT(*) FROM retail_sales;

--Q2 How many customers do we have?
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;

--Q3 Categories available in sales
SELECT DISTINCT category FROM retail_sales;
```

## Data analysis
1. SQL queries has been developed to answer following business questions.

```sql
--Sales & Revenue Analysis

--Q1 Write a SQL query to retrieve all columns for sales made on '2022-11-05'
SELECT * FROM retail_sales
WHERE sale_date = '2022-11-05';

--Q2 Write a SQL query to find all transactions where the total_sale is greater than 1000.
SELECT * FROM retail_sales
WHERE total_sale > 1000;

--Q3 Write a SQL query to calculate the total sales (total_sale) for each category.
SELECT category, SUM(total_sale) AS "Total sales", COUNT(*) AS "Total orders"
FROM retail_sales
GROUP BY category;

--Q4 Which customer had the highest single transaction value?
SELECT customer_id, transactions_id, category, quantity, total_sale
FROM retail_sales
ORDER BY total_sale DESC
LIMIT 1;

--Q5 Write a SQL query to calculate the best-selling month in each year. (Highest revenue generated month)

SELECT year, month, "total_sales"
FROM(
SELECT EXTRACT(YEAR FROM sale_date) AS "year",
TO_CHAR(sale_date, 'Month') AS "month",
SUM(total_sale) AS "total_sales",
RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY SUM(total_sale) DESC) AS "rank"
FROM retail_sales
GROUP BY EXTRACT(YEAR FROM sale_date), TO_CHAR(sale_date, 'Month'))S1
WHERE rank=1;

--Q6 Write a SQL query to find trend of order volume vs revenue per month? (difference between the previous month revenue)
SELECT month, order_volume, total_revenue,
LAG(total_revenue) OVER(ORDER BY month) AS prev_mon_revenue,
total_revenue - LAG(total_revenue) OVER(ORDER BY month) AS revenue_increase
FROM(SELECT TO_CHAR(sale_date, 'yyyy-mm') AS month,
COUNT(transactions_id) AS order_volume,
SUM(total_sale) AS total_revenue
FROM retail_sales
GROUP BY TO_CHAR(sale_date, 'yyyy-mm')
)AS mon_sales
ORDER BY month;

--Customer Behavior & Segmentation

--Q1 Write a SQL query to find the top 5 customers based on the highest total sales
SELECT customer_id, SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;

--Q2 Write a SQL query to find the customers who purchased items from all three categories
SELECT customer_id
FROM retail_sales
GROUP BY customer_id
HAVING COUNT(DISTINCT category) = 3;

--Q3 Write a SQL query to find the customer details and category of the items purchased.
SELECT customer_id,
STRING_AGG(DISTINCT category, ', ' ORDER BY category) AS categories
FROM retail_sales
GROUP BY customer_id;

--Q4 Which customers are repeat buyers (i.e., made purchases on more than one day)
SELECT customer_id, COUNT(DISTINCT sale_date) AS days_count
FROM retail_sales
GROUP BY customer_id
HAVING COUNT(DISTINCT sale_date) > 1;

--Q5 Write a SQL query to find the count of repeated customers
SELECT COUNT(*) AS customer_count
FROM(
SELECT customer_id
FROM retail_sales
GROUP BY customer_id
HAVING COUNT(DISTINCT sale_date) > 1
)repeated_customer;

--Q6 What is the average basket size (avg quantity per transaction) per customer? 
SELECT customer_id, ROUND(AVG(quantity)::numeric,2) AS average_quantity
FROM retail_sales
GROUP BY customer_id
ORDER BY average_quantity DESC;

--Product/Category Level Insights

--Q1 Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is atleast 4 in the month of Nov-2022
SELECT * FROM retail_sales
WHERE category = 'Clothing'
AND quantity >= 4
AND sale_date between '2022-11-01' AND '2022-11-30';

--Q2 Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.
SELECT ROUND(AVG(age),1) AS "Average age"
FROM retail_sales
WHERE category = 'Beauty';

--Q3 Write a SQL query to find the number of unique customers who purchased items from each category
SELECT category, COUNT(DISTINCT customer_id) AS "Customer count"
FROM retail_sales
GROUP BY category;

--Demographic Analysis

--Q1 Write a SQL query to find the total number of transactions (transactions_id) made by each gender in each category.
SELECT gender, category, COUNT(transactions_id) AS "Total orders"
FROM retail_sales
GROUP BY gender, category;

--Q2 Which gender spends more on average per transaction by category?
SELECT gender, category, ROUND(AVG(total_sale)::numeric,2) AS avg_per_transaction
FROM retail_sales
GROUP BY gender, category
ORDER BY gender, avg_per_transaction DESC;

--Q3 Write a SQL query to find the total transactions by age groups (e.g. <25, 25-40, 40+), gender, and by category.
SELECT gender, category,
CASE
WHEN age < 25 THEN 'Below 25'
WHEN age BETWEEN 25 AND 40 THEN '25-40'
ELSE 'Above 40'
END AS age_range,
SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY gender, category, age_range
ORDER BY gender, total_sales DESC;

--Time-Based & Shift Analysis

--Q1 Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)
WITH hourly_order AS(
SELECT *,
CASE
WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
WHEN EXTRACT(HOUR FROM sale_time) > 17 THEN 'Evening'
END AS shift
FROM retail_sales
)
SELECT shift, COUNT(*) AS total_orders
FROM hourly_order
GROUP BY shift;

--Q2 Which time of day (Morning, Afternoon, Evening) yields the highest revenue by category?
WITH hourly_order AS(
SELECT *,
CASE
WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
WHEN EXTRACT(HOUR FROM sale_time) > 17 THEN 'Evening'
END AS shift
FROM retail_sales
)
SELECT shift, category, SUM(total_sale) AS total_revenue
FROM hourly_order
GROUP BY shift, category
ORDER BY shift, total_revenue DESC;

--Q3 Compare total sales between weekdays vs weekends. 
SELECT 
CASE 
WHEN EXTRACT(DOW FROM sale_date) IN (0, 6) THEN 'Weekend'
ELSE 'Weekday'
END AS day_name,
SUM(total_sale) AS total_sales,
COUNT(DISTINCT customer_id) AS unique_customers,
COUNT(*) AS total_transactions
FROM retail_sales
GROUP BY day_name
ORDER BY day_name;

--Q4 Write a SQL query to calculate the total revenue for each day.
SELECT 
TO_CHAR(sale_date, 'Day') AS day_of_week,
SUM(total_sale) AS total_revenue
FROM retail_sales
GROUP BY TO_CHAR(sale_date, 'Day')
ORDER BY total_revenue DESC;
```

## Insights drawn




