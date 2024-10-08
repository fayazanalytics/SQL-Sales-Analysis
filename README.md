# Comprehensive Sales Analysis with SQL

# This project focuses on performing complex queries for sales analysis using SQL. It demonstrates various aspects of data retrieval and manipulation to gain meaningful business insights. The dataset consists of sales transactions, products, customers, categories, and employee information, and the queries are designed to analyze different aspects of the sales process.

# Project Overview

# The objective of this project is to showcase key SQL skills for sales data analysis, including:

# Calculating total sales and revenue.
# Identifying top-selling products.

# Tracking customer orders and behavior.
# Ranking products and customers by various metrics.
# Monitoring inventory and complaints.
# Key Queries and Insights

# 1. Total Quantity of Products Sold in May 2022
This query calculates the total quantity of products sold in May 2022 by joining the orders, order details, and products tables.

# My Code

SELECT SUM(od.quantity) AS total_quantity
FROM orders o 	
INNER JOIN order_details od ON o.order_id = od.order_id
INNER JOIN products p ON od.product_id = p.product_id
WHERE o.order_date >= '2022-05-01' AND o.order_date < '2022-06-01';

# 2. Revenue Generated by "Electronics" in December 2022
This query determines the total revenue generated by the "Electronics" category during December 2022.

# My Code
SELECT SUM(od.quantity * od.unit_price) AS total_revenue
FROM orders o
INNER JOIN order_details od ON o.order_id = od.order_id
INNER JOIN products p ON od.product_id = p.product_id
INNER JOIN categories c ON p.category_id = c.category_id
WHERE c.category_name = 'Electronics' AND o.order_date >= '2022-12-01' AND o.order_date < '2023-01-01';

# 3. Customer Details for Orders over $100 in May

This query returns the name and email of customers who placed orders greater than $100 in May 2022.

# My Code

SELECT c.name, c.email
FROM customers c
INNER JOIN orders o ON c.customer_id = o.customer_id
INNER JOIN order_details od ON o.order_id = od.order_id
INNER JOIN products p ON od.product_id = p.product_id
WHERE MONTH(o.order_date) = 5 AND p.price > 100;

# 4. Delivered Orders for Electronics

This query retrieves the name and address of customers who have ordered from the "Electronics" category and had their orders delivered.

# My Code

SELECT c.name, c.address
FROM customers c
INNER JOIN orders o ON c.customer_id = o.customer_id
INNER JOIN order_details od ON o.order_id = od.order_id
INNER JOIN products p ON od.product_id = p.product_id
INNER JOIN categories cat ON p.category_id = cat.category_id
WHERE cat.category_name = 'Electronics' AND o.order_status = 'Delivered';

# 5. Customer and Product Analysis
# This set of queries analyzes customers and products, including customers who haven't placed orders, products with no orders, and orders exceeding $100.


SELECT c.name, c.email FROM customers c LEFT JOIN orders o ON c.customer_id = o.customer_id;

SELECT p.product_name, p.price FROM products p RIGHT JOIN order_details od ON p.product_id = od.product_id;

# 6. Ranking Customers by Total Purchases

# This query ranks customers based on total purchase amount and total quantity purchased.

# My Code

SELECT Customer_Id, TotalAmountPaid, 
RANK() OVER(ORDER BY TotalAmountPaid DESC) AS _Rank 
FROM (SELECT Customer_Id, SUM(Amount_Paid) AS TotalAmountPaid FROM ProductSalesFact GROUP BY Customer_Id) AS T;

# 7. Ranking Products by Price and Inventory

# This query identifies the top-ranking products by price and by how long they've been in inventory.

# My Code

SELECT Product_Id, Price, Category_Id, 
DENSE_RANK() OVER(PARTITION BY Category_Id ORDER BY Price DESC) AS _rank 
FROM ProductDim 
WHERE _rank = 1;

# 8. Complaint Resolution Analysis

# This query ranks complaints based on how long they've remained unresolved.

# My Code

SELECT Complaint_Name, DATEDIFF(NOW(),Complaint_Date) AS TotalDaysNotResolved, 
RANK() OVER(PARTITION BY Complaint_Name ORDER BY DATEDIFF(NOW(),Complaint_Date) DESC) AS _rank 
FROM Complaints WHERE Resolved != 'Resolved';
# Key Features
# Data Joins: Various types of joins, including INNER JOIN, LEFT JOIN, and RIGHT JOIN are used to combine data from multiple tables.
# Window Functions: Ranking customers and products using RANK() and DENSE_RANK().
# Aggregation: Calculating totals, averages, and sums for sales and complaints.
# Filtering: Conditions to retrieve only the necessary data for analysis (e.g., dates, categories).
# Date Functions: Handling date ranges and calculating differences in days.
# Tools and Technologies

# Conclusion
This project demonstrates the capability of SQL in handling comprehensive sales data, offering insights into key metrics like sales quantity, revenue, product performance, customer behavior, and complaint resolution. By leveraging SQL joins, ranking functions, and aggregation, this project highlights the practical applications of SQL in business data analysis.

