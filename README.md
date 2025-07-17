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

## 1. Setting up a database:
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

## 2.Data cleaning:
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



