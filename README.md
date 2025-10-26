# MySQL ECommerce Data Analysis

This project analyzes retail business data using three main tables: **Customers**, **Products**, and **Transactions**. The data was originally provided in CSV format and imported into MySQL. Below are step-by-step SQL queries commonly used for business data analysis.

---

## Table Structure (Adjust column names as per your schema)

### Customers
- `customer_id` (Primary Key)
- `customer_name`
- `email`
- `address`
- other relevant columns...

### Products
- `product_id` (Primary Key)
- `product_name`
- `category`
- `price`
- other relevant columns...

### Transactions
- `transaction_id` (Primary Key)
- `customer_id` (Foreign Key)
- `product_id` (Foreign Key)
- `quantity`
- `amount`
- `transaction_date`
- other relevant columns...

---

## SQL Analysis Queries

> Copy and run these queries in your MySQL environment. 
> Adjust table and column names as needed.

1. **Total number of customers**
