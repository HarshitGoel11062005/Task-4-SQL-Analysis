MySQL Retail Data Analysis
This project analyzes retail business data using three main tables: Customers, Products, and Transactions. The data was provided in CSV format and imported into MySQL. Below are step-by-step SQL queries commonly used for business data analysis.

Table Structure (Adjust column names as per your schema)
Customers:

customer_id (Primary Key)

customer_name

email

address

other relevant columns...

Products:

product_id (Primary Key)

product_name

category

price

other relevant columns...

Transactions:

transaction_id (Primary Key)

customer_id (Foreign Key)

product_id (Foreign Key)

quantity

amount

transaction_date

other relevant columns...

SQL Analysis Queries
Copy and run these queries in your MySQL environment. Adjust table and column names as needed.

Total number of customers

sql
SELECT COUNT(*) AS total_customers FROM Customers;
List products sorted by price (descending)

sql
SELECT * FROM Products ORDER BY price DESC;
Total number of transactions

sql
SELECT COUNT(*) AS total_transactions FROM Transactions;
Calculate total sales

sql
SELECT SUM(amount) AS total_sales FROM Transactions;
Top 10 customers by total purchase amount

sql
SELECT c.customer_id, c.customer_name, SUM(t.amount) AS total_spent
FROM Customers c
JOIN Transactions t ON c.customer_id = t.customer_id
GROUP BY c.customer_id, c.customer_name
ORDER BY total_spent DESC
LIMIT 10;
Best-selling products by units sold

sql
SELECT p.product_id, p.product_name, SUM(t.quantity) AS total_units_sold
FROM Products p
JOIN Transactions t ON p.product_id = t.product_id
GROUP BY p.product_id, p.product_name
ORDER BY total_units_sold DESC;
Customers who haven't made any purchases

sql
SELECT *
FROM Customers
WHERE customer_id NOT IN (SELECT DISTINCT customer_id FROM Transactions);
Monthly sales summary

sql
SELECT YEAR(t.transaction_date) AS year, MONTH(t.transaction_date) AS month, SUM(t.amount) AS monthly_sales
FROM Transactions t
GROUP BY YEAR(t.transaction_date), MONTH(t.transaction_date)
ORDER BY year, month;
Average transaction amount

sql
SELECT AVG(amount) AS avg_transaction_amount FROM Transactions;
Distribution of transactions per product

sql
SELECT p.product_name, COUNT(t.transaction_id) AS num_transactions
FROM Products p
JOIN Transactions t ON p.product_id = t.product_id
GROUP BY p.product_name
ORDER BY num_transactions DESC;
Repeat customers (more than one transaction)

sql
SELECT c.customer_id, c.customer_name, COUNT(t.transaction_id) AS transaction_count
FROM Customers c
JOIN Transactions t ON c.customer_id = t.customer_id
GROUP BY c.customer_id, c.customer_name
HAVING transaction_count > 1;
Total transaction value per product category

sql
SELECT p.category, SUM(t.amount) AS total_sales
FROM Products p
JOIN Transactions t ON p.product_id = t.product_id
GROUP BY p.category
ORDER BY total_sales DESC;
Skip or adapt if no category column in your Products table.

Sales trend for top 3 products by units sold

sql
SELECT p.product_name, YEAR(t.transaction_date) AS year, MONTH(t.transaction_date) AS month, SUM(t.amount) AS monthly_sales
FROM Products p
JOIN Transactions t ON p.product_id = t.product_id
GROUP BY p.product_name, YEAR(t.transaction_date), MONTH(t.transaction_date)
HAVING p.product_id IN (
    SELECT product_id FROM (
        SELECT product_id, SUM(quantity) AS total_units
        FROM Transactions
        GROUP BY product_id
        ORDER BY total_units DESC
        LIMIT 3
    ) AS TopProducts
)
ORDER BY p.product_name, year, month;
Customers with highest average purchase size (top 10)

sql
SELECT c.customer_id, c.customer_name, AVG(t.amount) AS avg_purchase
FROM Customers c
JOIN Transactions t ON c.customer_id = t.customer_id
GROUP BY c.customer_id, c.customer_name
ORDER BY avg_purchase DESC
LIMIT 10;
Transactions with unusually high amounts (outliers)

sql
SELECT *
FROM Transactions
WHERE amount > (SELECT AVG(amount) + 3 * STDDEV(amount) FROM Transactions);
