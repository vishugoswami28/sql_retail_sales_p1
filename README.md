# SQL Project (Retail Sales Analysis)

## Overview

**Title**: Retail Sales Analysis  
**Database**: sql_project_p1

This project showcases practical SQL skills commonly applied by data analysts to explore, clean, and analyze retail sales data. It includes creating a retail sales database, conducting exploratory data analysis (EDA), and solving business-related questions through SQL queries. The project is well-suited for beginners in data analysis who want to strengthen their SQL foundation through hands-on practice.

## Objectives

1. **Database Setup**: Build and populate a retail sales database using the provided dataset.
2. **Data Cleaning**: Detect and handle missing or null values to ensure data quality.
3. **Exploratory Data Analysis (EDA)**: Conduct exploratory analysis to uncover patterns and understand key characteristics of the dataset.
4. **Business Insights**: Apply SQL queries to answer business questions and generate actionable insights from the sales data.

## Structure

### 1. Database Setup

- **Database Creation**: The project begins with creating a database named `sql_project_p1`.
- **Table Creation**: A table called retail_sales is created to store the dataset. It includes key fields such as transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
CREATE DATABASE sql_project_p1;

CREATE TABLE retail_sales
            (
               transactions_id INT PRIMARY KEY,
               sale_date DATE,
               sale_time TIME,	
               customer_id INT,
               gender VARCHAR(15),
               age INT,
               category VARCHAR(15),
               quantity INT,
               price_per_unit FLOAT,
               cogs FLOAT,
               total_sale FLOAT
            );
```

### 2. Data Exploration & Cleaning 

- **Record Count**: Calculate the total number of records in the dataset.
- **Customer Count**: Determine the number of unique customers.
- **Category Count**: List all distinct product categories.
- **Null Value Check**: Identify and remove records containing null or missing values.

```sql
SELECT COUNT(*) as total_sale FROM retail_sales;
SELECT COUNT (DISTINCT customer_id) as unique_customer FROM retail_sales;
SELECT DISTINCT category FROM retail_sales;

SELECT * FROM retail_sales
WHERE 
    transactions_id IS NULL OR sale_date IS NULL OR sale_time IS NULL OR
    customer_id IS NULL OR gender IS NULL OR age IS NULL OR category IS NULL
    OR quantity IS NULL OR price_per_unit IS NULL OR  cogs IS NULL OR total_sale IS NULL;

DELETE FROM retail_sales
WHERE 
    transactions_id IS NULL OR sale_date IS NULL OR sale_time IS NULL OR
    customer_id IS NULL OR gender IS NULL OR age IS NULL OR category IS NULL
    OR quantity IS NULL OR price_per_unit IS NULL OR  cogs IS NULL OR total_sale IS NULL;
```

### 3. Exploratory Data Analysis (EDA)

The following SQL queries were developed to answer specific business questions:

1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05**:
```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is 4 or more in the month of Nov-2022**:
```sql
SELECT *
FROM retail_sales
WHERE category = 'Clothing'
AND EXTRACT (MONTH FROM sale_date) = 11
AND EXTRACT (YEAR FROM sale_date) = 2022
AND quantity >= 4;
```

3. **Write a SQL query to calculate the total sales (total_sale) for each category.**:
```sql
SELECT 
     category,
     SUM(total_sale) as Net_Sale,
FROM retail_sales
GROUP BY 1;
```

4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**:
```sql
SELECT   
  ROUND(AVG (age), 2) as customer_avg_age
FROM retail_sales
WHERE category = 'Beauty';
```

5. **Write a SQL query to find all transactions where the total_sale is greater than 1000.**:
```sql
SELECT * 
FROM retail_sales
WHERE total_sale >1000;

```

6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**:
```sql
SELECT 
     category,
	 gender,
	 COUNT(transactions_id) AS total_transaction
FROM retail_sales
GROUP BY 1,2
ORDER BY 1,2;
```

7. **Write a SQL query to calculate the average sale for each month. Find out the best-selling month in each year**:
```sql
SELECT
     year,
	 month,
	 avg_sale
FROM
(
SELECT
   EXTRACT(YEAR FROM sale_date) AS year,
   EXTRACT(MONTH FROM sale_date) AS month,
   ROUND(AVG(total_sale) :: numeric, 3) AS avg_sale,
   RANK ()OVER(PARTITION BY EXTRACT(YEAR FROM sale_date)ORDER BY AVG(total_sale)DESC) AS rank
FROM retail_sales
GROUP BY 1,2
) as t1
WHERE rank = 1;
```

8. **Write a SQL query to find the top 5 customers based on the highest total sales **:
```sql
SELECT 
    customer_id,
    SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;

```

9. **Write a SQL query to find the number of unique customers who purchased items from each category.**:
```sql
SELECT 
    category,
    COUNT (DISTINCT customer_id) AS unique_customer
FROM retail_sales
GROUP BY 1;
```

10. **Write a SQL query to create each shift and number of orders (Example: Morning <12, Afternoon Between 12 & 17, Evening >17)**:
```sql
WITH hourly_sale
AS
(
SELECT *,
     CASE 
        WHEN EXTRACT (HOUR FROM sale_time)<12 THEN 'Morning'
        WHEN EXTRACT (HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
		ELSE 'Evening'
	END AS shift
FROM retail_sales
)
SELECT
     shift,
	 COUNT(*) AS total_orders
FROM hourly_sale
GROUP BY shift;
```

## Findings

- **Customer Demographics**: Customers represent a wide range of age groups, with purchases spread across multiple categories such as Clothing and Beauty.
- **High-Value Transactions**: Several sales exceeded 1,000 in total amount, highlighting premium or bulk purchases..
- **Sales Trends**: Monthly analysis reveals noticeable fluctuations in sales, helping to identify peak and low seasons.
- **Customer Insights**: The analysis highlights top-spending customers as well as the most in-demand product categories.

## Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales trends across different months and shifts.
- **Customer Insights**: Reports on top customers and unique customer counts per category.

## Conclusion

This project provides a well-rounded introduction to SQL for aspiring data analysts, encompassing database setup, data cleaning, exploratory data analysis, and business-focused querying. The insights gained—ranging from sales patterns to customer behavior and product performance—demonstrate how SQL can support data-driven decision-making in a retail context.









