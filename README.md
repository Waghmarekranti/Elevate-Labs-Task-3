# Elevate-Labs-Task-3

Project Overview

This project contains SQL queries performed on the ecommercepk1 dataset under the database Elevate_Lab.
The goal of this mini–project is to practice and demonstrate SQL concepts such as:

Basic SELECT queries

Applying WHERE filters

Sorting with ORDER BY

Grouping data using GROUP BY

Joining multiple tables (INNER / LEFT / RIGHT JOIN)

Writing subqueries

Using aggregate functions (SUM, AVG, MAX, MIN)

Creating views for analysis

Applying indexes for query optimization

Code Explation:
. CREATE DATABASE
CREATE DATABASE Elevate_Lab;

What it does:

✅ 1.Creates a new database named Elevate_Lab.

Details:

A database is like a container for tables.

Used to keep all project-related tables in one place.

If the database already exists, MySQL may show an error unless you use:

CREATE DATABASE IF NOT EXISTS Elevate_Lab;

✅ 2. USE Database
USE Elevate_LLab;

What it does:

Selects the database where you want to run all upcoming SQL commands.

Details:

Without USE, SQL does not know which database's tables you are trying to access.

✅ 3. SELECT All Data
SELECT * FROM ecommercepk1;

What it does:

Fetches all rows and all columns from the table.

Details:

* means "every column."

Useful for checking data, but not recommended in production (slower + unnecessary data).

✅ 4. WHERE Clause (Filter Results)
SELECT day, amount_spent
FROM ecommercepk1
WHERE amount_spent > 2000;

What it does:

Shows only those rows where amount_spent is greater than 2000.

Why it’s used:

To filter data using conditions.

Works with operators like >, <, >=, =, !=.

Note:

Rows with NULL in amount_spent will not be included (because NULL cannot be compared with >).

✅ 5. ORDER BY (Sort Data)
SELECT day, amount_spent
FROM ecommercepk1
ORDER BY amount_spent DESC;

What it does:

Sorts the data in descending order of spending.

Details:

DESC = highest to lowest

ASC = lowest to highest (default)

✅ 6. GROUP BY (Aggregate per Day)
SELECT day, SUM(amount_spent) AS total_spent
FROM ecommercepk1
GROUP BY day
ORDER BY day;

What it does:

Groups rows by day.

Calculates total spending per day using SUM().

Details:

All non-aggregated columns in SELECT must appear in GROUP BY.

Used for reporting, analytics, dashboards.

✅ 7. Create Second Table
CREATE TABLE page_details (
    page_name VARCHAR(50),
    category VARCHAR(50)
);

What it does:

Creates a new table to store page names and their categories.

Details:

VARCHAR(50) stores text up to 50 characters.

This table will later be joined with ecommercepk1.

✅ 8. Insert Data
INSERT INTO page_details (page_name, category)
VALUES
('censored', 'Shopping'),
('ABC Store', 'Grocery'),
('XYZ Ads', 'Electronics');

What it does:

Adds three rows into page_details.

Details:

You can insert multiple rows at once.

Data types must match column types.

✅ 9. INNER JOIN
SELECT e.day, e.amount_spent, p.category
FROM ecommercepk1 e
INNER JOIN page_details p
ON e.page_name = p.page_name;

What it does:

Returns only the rows where page_name matches in both tables.

Details:

Shows common data (intersection).

If page_name does not match, the row is not included.

✅ 10. LEFT JOIN
SELECT e.*, p.category
FROM ecommercepk1 e
LEFT JOIN page_details p
ON e.page_name = p.page_name;

What it does:

Returns all rows from ecommercepk1, even if category is missing.

Details:

Non-matching rows will show category = NULL.

Useful to identify missing category mappings.

✅ 11. RIGHT JOIN
SELECT e.*, p.category
FROM ecommercepk1 e
RIGHT JOIN page_details p
ON e.page_name = p.page_name;

What it does:

Returns all rows from page_details, even if they do not exist in ecommercepk1.

Details:

Opposite of LEFT JOIN.

Less commonly used; LEFT JOIN is preferred.

✅ 12. SUBQUERY
SELECT *
FROM ecommercepk1
WHERE amount_spent > (SELECT AVG(amount_spent) FROM ecommercepk1);

What it does:

Shows rows where amount_spent is greater than the average of the table.

Details:

(SELECT AVG(...)) returns a single value.

The outer query compares each row to this value.

✅ 13. Aggregate Functions
SUM
SELECT SUM(amount_spent) AS total_spending
FROM ecommercepk1;


→ Returns total amount spent.

AVG
SELECT AVG(amount_spent) AS avg_spending
FROM ecommercepk1;


→ Returns average spending.

MAX and MIN
SELECT MAX(amount_spent), MIN(amount_spent)
FROM ecommercepk1;


→ Returns highest and lowest spending amounts.

Details:

These functions ignore NULL values.

✅ 14. Create View
CREATE VIEW high_spend_days AS
SELECT page_name, day, amount_spent
FROM ecommercepk1
WHERE amount_spent > 2000;

What it does:

Creates a virtual table showing high-spend rows only.

SELECT * FROM high_spend_days;


→ Used to view data inside the view.

Details:

Views save time by storing commonly used queries.

They do not store data physically.

✅ 15. Create Index
CREATE INDEX idx_day ON ecommercepk1 (day(10)); 
CREATE INDEX idx_amount ON ecommercepk1 (amount_spent);

Purpose:

Indexes improve the speed of searches, sorting, and filtering.

Details:

idx_day → makes searching by day faster

idx_amount → makes filtering/order by amount faster

Indexes store sorted copies of columns internally

WARNING:

Too many indexes slow down INSERT/UPDATE operations.

✅ 16. Show Indexes
SHOW INDEXES FROM ecommercepk1;

What it does:

Displays all indexes on the table.
