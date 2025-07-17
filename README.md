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
For this project a database is created named project_1 and a table named retail_sales is created under itwith required colums and the it's data type. Table consists of 11 columns.
Columns present in the table: transactions ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

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

## 2.Data Cleaning:


