# Retail Sales Analysis SQL Project

## Project Overview

**Project Title**: Retail Sales Analysis  
**Level**: Beginner  
**Database**: `p1_retail_db`

This project is designed to demonstrate SQL skills by analyzing retail sales data. The goal is to practice creating a database, cleaning and exploring the data, and answering relevant business questions using SQL.

## Objectives

1. **Database Setup**: Build and populate a retail sales database using the provided sales data.
2. **Data Cleaning**: Detect and remove any records with missing or invalid data.
3. **Exploratory Data Analysis (EDA)**: Analyze the data to uncover patterns and insights.
4. **Business Analysis**: Use SQL to answer key business questions and extract valuable insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The first step involves creating a database called `p1_retail_db`.
- **Table Creation**: The main table, `retail_sales`, is structured to store sales data, including transaction ID, sale date, customer details, and financial information like price per unit and total sale amount.

```sql
CREATE DATABASE p1_retail_db;

CREATE TABLE retail_sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,	
    sale_time TIME,
    customer_id INT,	
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,	
    cogs FLOAT,
    total_sale FLOAT
);
```

2. Data Exploration & Cleaning
Record Count: Calculate the total number of records in the dataset.

Customer Count: Determine how many unique customers are in the dataset.

Category Count: List all the unique product categories.

Null Value Check: Identify and remove any records with missing data.
```sql

SELECT COUNT(*) FROM retail_sales;
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
SELECT DISTINCT category FROM retail_sales;

SELECT * FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

DELETE FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL; 
```
3. Data Analysis & Findings
The following SQL queries are used to answer business-related questions:

   a).Retrieve all sales data for '2022-11-05':
```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';

```
b). Find transactions in the 'Clothing' category with more than 4 items sold in Nov-2022:

```sql
SELECT 
  *
FROM retail_sales
WHERE 
    category = 'Clothing'
    AND 
    TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
    AND
    quantity >= 4

```
c). Calculate the total sales for each product category:
```sql
SELECT 
    category,
    SUM(total_sale) as net_sale,
    COUNT(*) as total_orders
FROM retail_sales
GROUP BY 1

```
d). Find the average age of customers who bought products in the 'Beauty' category:

```sql
SELECT
    ROUND(AVG(age), 2) as avg_age
FROM retail_sales
WHERE category = 'Beauty'

```
 e).Identify transactions with total sales greater than 1000:

```sql
SELECT * FROM retail_sales
WHERE total_sale > 1000

```
f).Calculate the number of transactions by gender for each category:

```sql
SELECT 
    category,
    gender,
    COUNT(*) as total_trans
FROM retail_sales
GROUP 
    BY 
    category,
    gender
ORDER BY 1

```
g).Calculate the average sale for each month and identify the best-selling month of the year
```sql
SELECT 
       year,
       month,
    avg_sale
FROM 
(    
SELECT 
    EXTRACT(YEAR FROM sale_date) as year,
    EXTRACT(MONTH FROM sale_date) as month,
    AVG(total_sale) as avg_sale,
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank
FROM retail_sales
GROUP BY 1, 2
) as t1
WHERE rank = 1

```
h).Find the top 5 customers based on total sales:

```sql
SELECT 
    customer_id,
    SUM(total_sale) as total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5

```
i).Find the number of unique customers who bought items from each category:

```sql
SELECT 
    category,    
    COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM retail_sales
GROUP BY category

```
Classify transactions based on sale time (Morning, Afternoon, Evening):

```sql
WITH hourly_sale
AS
(
SELECT *,
    CASE
        WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END as shift
FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) as total_orders    
FROM hourly_sale
GROUP BY shift

```
Findings
Customer Demographics: The dataset represents a diverse range of customers, with varied age groups and product preferences.

High-Value Transactions: Some transactions exceed 1000 in total sale, indicating high-value purchases.

Sales Trends: The monthly sales analysis provides valuable insights into peak and off-peak periods for the business.

Customer Insights: The project identifies top customers and their spending patterns, along with the most frequently purchased categories.

Conclusion
This project serves as an excellent introduction to SQL for data analysts, focusing on database creation, data cleaning, exploratory data analysis, and SQL-based business analysis. The findings offer useful insights for businesses seeking to understand their sales patterns, customer behavior, and product performance.

How to Use
Clone the Repository: Clone this project repository from GitHub.

Set Up the Database: Run the SQL scripts provided in the database_setup.sql file to create and populate the database.

Run the Queries: Use the SQL queries provided in the analysis_queries.sql file to perform your analysis.

Explore and Modify: Feel free to modify the queries to explore different aspects of the dataset or answer additional business questions.

yaml
Copy
Edit



