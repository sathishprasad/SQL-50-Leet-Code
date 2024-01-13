# Day 4 of a 10-Day Learning Challenge 🚀

### Problem 16: Average Selling Price 

````
Table: Prices
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| start_date    | date    |
| end_date      | date    |
| price         | int     |
+---------------+---------+
(product_id, start_date, end_date) is the primary key (combination of columns with unique values) for this table.
Each row of this table indicates the price of the product_id in the period from start_date to end_date.
For each product_id there will be no two overlapping periods. That means there will be no two intersecting periods for the same product_id.
 

Table: UnitsSold
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| purchase_date | date    |
| units         | int     |
+---------------+---------+
This table may contain duplicate rows.
Each row of this table indicates the date, units, and product_id of each product sold. 
 

Write a solution to find the average selling price for each product. average_price should be rounded to 2 decimal places.

Return the result table in any order.

The result format is in the following example.

Example 1:

Input: 
Prices table:
+------------+------------+------------+--------+
| product_id | start_date | end_date   | price  |
+------------+------------+------------+--------+
| 1          | 2019-02-17 | 2019-02-28 | 5      |
| 1          | 2019-03-01 | 2019-03-22 | 20     |
| 2          | 2019-02-01 | 2019-02-20 | 15     |
| 2          | 2019-02-21 | 2019-03-31 | 30     |
+------------+------------+------------+--------+
UnitsSold table:
+------------+---------------+-------+
| product_id | purchase_date | units |
+------------+---------------+-------+
| 1          | 2019-02-25    | 100   |
| 1          | 2019-03-01    | 15    |
| 2          | 2019-02-10    | 200   |
| 2          | 2019-03-22    | 30    |
+------------+---------------+-------+
Output: 
+------------+---------------+
| product_id | average_price |
+------------+---------------+
| 1          | 6.96          |
| 2          | 16.96         |
+------------+---------------+
Explanation: 
Average selling price = Total Price of Product / Number of products sold.
Average selling price for product 1 = ((100 * 5) + (15 * 20)) / 115 = 6.96
Average selling price for product 2 = ((200 * 15) + (30 * 30)) / 230 = 16.96
````
### Problem 16 Solution and Explanation:

```sql
SELECT product_id, 
       IFNULL(ROUND(SUM(total_price) / SUM(units), 2), 0) AS average_price
FROM (
    SELECT  p.product_id,
            u.units,
            p.price * u.units AS total_price
    FROM Prices AS p
    LEFT JOIN UnitsSold AS u
    ON p.product_id = u.product_id AND 
       u.purchase_date BETWEEN p.start_date AND p.end_date
) AS t
GROUP BY product_id;
```

**Let's break down this SQL query:**

- **`SELECT product_id, IFNULL(ROUND(SUM(total_price) / SUM(units), 2), 0) AS average_price:`**
  - This part of the query selects the `product_id` and calculates the `average_price` for each product. The `IFNULL` function is used to handle cases where the denominator (`SUM(units)`) might be zero, preventing division by zero errors. The `ROUND` function is used to round the result to two decimal places.

- **`FROM (... ) AS t:`**
  - This is a subquery that combines data from the Prices (`p`) and UnitsSold (`u`) tables using a `LEFT JOIN`. It matches rows where the `product_id` is the same, and the `purchase_date` falls within the range defined by `start_date` and `end_date`. The result includes columns such as `product_id`, `units`, and `total_price` (calculated as `price * units`).

- **`GROUP BY product_id:`**
  - This groups the results by `product_id`, so the subsequent aggregate functions apply to each unique product.

**In summary,** this SQL query calculates the average price for each product, considering the units sold and their corresponding prices. It uses a subquery with a `LEFT JOIN` to match the relevant data from Prices and UnitsSold tables, and then it aggregates the results by product_id. The `IFNULL` function handles cases where there are no units sold for a particular product, ensuring that division by zero is avoided.

---

### Problem 17: Project Employees I

````
Table: Project

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| project_id  | int     |
| employee_id | int     |
+-------------+---------+
(project_id, employee_id) is the primary key of this table.
employee_id is a foreign key to Employee table.
Each row of this table indicates that the employee with employee_id is working on the project with project_id.
 

Table: Employee

+------------------+---------+
| Column Name      | Type    |
+------------------+---------+
| employee_id      | int     |
| name             | varchar |
| experience_years | int     |
+------------------+---------+
employee_id is the primary key of this table. It's guaranteed that experience_years is not NULL.
Each row of this table contains information about one employee.
 

Write an SQL query that reports the average experience years of all the employees for each project, rounded to 2 digits.

Return the result table in any order.

The query result format is in the following example.

Example 1:

Input: 
Project table:
+-------------+-------------+
| project_id  | employee_id |
+-------------+-------------+
| 1           | 1           |
| 1           | 2           |
| 1           | 3           |
| 2           | 1           |
| 2           | 4           |
+-------------+-------------+
Employee table:
+-------------+--------+------------------+
| employee_id | name   | experience_years |
+-------------+--------+------------------+
| 1           | Khaled | 3                |
| 2           | Ali    | 2                |
| 3           | John   | 1                |
| 4           | Doe    | 2                |
+-------------+--------+------------------+
Output: 
+-------------+---------------+
| project_id  | average_years |
+-------------+---------------+
| 1           | 2.00          |
| 2           | 2.50          |
+-------------+---------------+
Explanation: The average experience years for the first project is (3 + 2 + 1) / 3 = 2.00 and for the second project is (3 + 2) / 2 = 2.50
````
### Problem 17 Solution and Explanation:

```sql
SELECT p.project_id, ROUND(AVG(e.experience_years), 2) AS average_years
FROM Project AS p
LEFT JOIN Employee AS e
ON p.employee_id = e.employee_id
GROUP BY 1;
```

**Let's break down this SQL query:**

- **`SELECT p.project_id, ROUND(AVG(e.experience_years), 2) AS average_years:`**
  - This part of the query selects the `project_id` and calculates the rounded average of `experience_years` for each project. The `ROUND` function is used to round the result to two decimal places.

- **`FROM Project AS p:`**
  - Specifies the main table from which we want to retrieve data, and in this case, it's the Project table. It is given the alias `p` for brevity.

- **`LEFT JOIN Employee AS e ON p.employee_id = e.employee_id:`**
  - This is a `LEFT JOIN` clause that combines data from the Project table (aliased as `p`) with the Employee table (aliased as `e`). It matches rows where the employee IDs (`employee_id`) in both tables are equal.

- **`GROUP BY 1:`**
  - This groups the results by the first selected column (`project_id`). The `AVG` function then calculates the average of `experience_years` for each project.

**In summary,** this SQL query retrieves the `project_id` and the rounded average of `experience_years` for each project. It uses a `LEFT JOIN` to include all projects, even those without corresponding employee records. The results are then grouped by `project_id`, and the average experience years are calculated for each group.

---

### Problem 18: Percentage of Users Attended a Contest

````
Table: Users
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| user_id     | int     |
| user_name   | varchar |
+-------------+---------+
user_id is the primary key (column with unique values) for this table.
Each row of this table contains the name and the id of a user.
 

Table: Register
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| contest_id  | int     |
| user_id     | int     |
+-------------+---------+
(contest_id, user_id) is the primary key (combination of columns with unique values) for this table.
Each row of this table contains the id of a user and the contest they registered into.
 

Write a solution to find the percentage of the users registered in each contest rounded to two decimals.

Return the result table ordered by percentage in descending order. In case of a tie, order it by contest_id in ascending order.

The result format is in the following example.

Example 1:

Input: 
Users table:
+---------+-----------+
| user_id | user_name |
+---------+-----------+
| 6       | Alice     |
| 2       | Bob       |
| 7       | Alex      |
+---------+-----------+
Register table:
+------------+---------+
| contest_id | user_id |
+------------+---------+
| 215        | 6       |
| 209        | 2       |
| 208        | 2       |
| 210        | 6       |
| 208        | 6       |
| 209        | 7       |
| 209        | 6       |
| 215        | 7       |
| 208        | 7       |
| 210        | 2       |
| 207        | 2       |
| 210        | 7       |
+------------+---------+
Output: 
+------------+------------+
| contest_id | percentage |
+------------+------------+
| 208        | 100.0      |
| 209        | 100.0      |
| 210        | 100.0      |
| 215        | 66.67      |
| 207        | 33.33      |
+------------+------------+
Explanation: 
All the users registered in contests 208, 209, and 210. The percentage is 100% and we sort them in the answer table by contest_id in ascending order.
Alice and Alex registered in contest 215 and the percentage is ((2/3) * 100) = 66.67%
Bob registered in contest 207 and the percentage is ((1/3) * 100) = 33.33%
````
### Problem 18 Solution and Explanation:

```sql
SELECT r.contest_id, ROUND(COUNT(r.contest_id) / (SELECT COUNT(*) FROM Users) * 100, 2) AS percentage
FROM Users AS u
INNER JOIN Register AS r
ON u.user_id = r.user_id
GROUP BY 1
ORDER BY 2 DESC, 1 ASC;
```

**Let's break down this SQL query:**

- **`SELECT r.contest_id, ROUND(COUNT(r.contest_id) / (SELECT COUNT(*) FROM Users) * 100, 2) AS percentage:`**
  - This part of the query selects the `contest_id` and calculates the percentage of users registered for each contest. The percentage is calculated as the count of registrations for a specific contest divided by the total count of users, multiplied by 100. The result is rounded to two decimal places using the `ROUND` function.

- **`FROM Users AS u INNER JOIN Register AS r ON u.user_id = r.user_id:`**
  - Specifies the main tables involved in the query. It uses an `INNER JOIN` between the Users table (aliased as `u`) and the Register table (aliased as `r`) based on matching user IDs (`user_id`).

- **`GROUP BY 1:`**
  - This groups the results by the first selected column (`contest_id`). The `COUNT` function then calculates the number of registrations for each contest.

- **`ORDER BY 2 DESC, 1 ASC;`**
  - Orders the result set first by the second selected column (`percentage`) in descending order (`DESC`), and then by the first selected column (`contest_id`) in ascending order (`ASC`).

**In summary,** this SQL query retrieves the `contest_id` and the percentage of users registered for each contest. It uses an `INNER JOIN` to match user registrations with user information, calculates the percentage based on the total number of users, and then orders the results by percentage in descending order and contest ID in ascending order.

----

### Problem 19: Queries Quality and Percentage

````
Table: Queries
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| query_name  | varchar |
| result      | varchar |
| position    | int     |
| rating      | int     |
+-------------+---------+
This table may have duplicate rows.
This table contains information collected from some queries on a database.
The position column has a value from 1 to 500.
The rating column has a value from 1 to 5. Query with rating less than 3 is a poor query.
 

We define query quality as:

The average of the ratio between query rating and its position.

We also define poor query percentage as:

The percentage of all queries with rating less than 3.

Write a solution to find each query_name, the quality and poor_query_percentage.

Both quality and poor_query_percentage should be rounded to 2 decimal places.

Return the result table in any order.

The result format is in the following example.

Example 1:

Input: 
Queries table:
+------------+-------------------+----------+--------+
| query_name | result            | position | rating |
+------------+-------------------+----------+--------+
| Dog        | Golden Retriever  | 1        | 5      |
| Dog        | German Shepherd   | 2        | 5      |
| Dog        | Mule              | 200      | 1      |
| Cat        | Shirazi           | 5        | 2      |
| Cat        | Siamese           | 3        | 3      |
| Cat        | Sphynx            | 7        | 4      |
+------------+-------------------+----------+--------+
Output: 
+------------+---------+-----------------------+
| query_name | quality | poor_query_percentage |
+------------+---------+-----------------------+
| Dog        | 2.50    | 33.33                 |
| Cat        | 0.66    | 33.33                 |
+------------+---------+-----------------------+
Explanation: 
Dog queries quality is ((5 / 1) + (5 / 2) + (1 / 200)) / 3 = 2.50
Dog queries poor_ query_percentage is (1 / 3) * 100 = 33.33

Cat queries quality equals ((2 / 5) + (3 / 3) + (4 / 7)) / 3 = 0.66
Cat queries poor_ query_percentage is (1 / 3) * 100 = 33.33
````
### Problem 19 Solution and Explanation:

```sql
SELECT query_name, 
       ROUND(SUM(quality) / COUNT(*), 2) AS quality,
       ROUND((SUM(CASE WHEN rating < 3 THEN 1 ELSE 0 END) / COUNT(*)) * 100, 2) AS poor_query_percentage
FROM (
    SELECT *, rating / position AS quality
    FROM Queries
    WHERE query_name IS NOT NULL
) AS t1
GROUP BY query_name;
```

**Let's break down this SQL query:**

- **`SELECT query_name, ROUND(SUM(quality) / COUNT(*), 2) AS quality, ... :`**
  - This part of the query selects the `query_name`, calculates the rounded average of `quality`, and calculates the percentage of poor queries. The `quality` is computed as the sum of `rating / position` for each query, and the `poor_query_percentage` is the percentage of queries with a rating less than 3.

- **`FROM (... ) AS t1:`**
  - This is a subquery that selects all columns from the Queries table where `query_name` is not NULL. It calculates the `quality` as `rating / position`.

- **`GROUP BY query_name;`**
  - Groups the results by `query_name` to perform aggregate functions on each unique query.

**In summary,** this SQL query retrieves the `query_name`, calculates the average `quality` (sum of `rating / position` divided by count) for each query, and computes the percentage of poor queries (with a rating less than 3). The results are grouped by `query_name`.

---

### Problem 20: Monthly Transactions I

````
Table: Transactions
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| country       | varchar |
| state         | enum    |
| amount        | int     |
| trans_date    | date    |
+---------------+---------+
id is the primary key of this table.
The table has information about incoming transactions.
The state column is an enum of type ["approved", "declined"].
 
Write an SQL query to find for each month and country, the number of transactions and their total amount, the number of approved transactions and their total amount.

Return the result table in any order.

The query result format is in the following example.

Example 1:

Input: 
Transactions table:
+------+---------+----------+--------+------------+
| id   | country | state    | amount | trans_date |
+------+---------+----------+--------+------------+
| 121  | US      | approved | 1000   | 2018-12-18 |
| 122  | US      | declined | 2000   | 2018-12-19 |
| 123  | US      | approved | 2000   | 2019-01-01 |
| 124  | DE      | approved | 2000   | 2019-01-07 |
+------+---------+----------+--------+------------+
Output: 
+----------+---------+-------------+----------------+--------------------+-----------------------+
| month    | country | trans_count | approved_count | trans_total_amount | approved_total_amount |
+----------+---------+-------------+----------------+--------------------+-----------------------+
| 2018-12  | US      | 2           | 1              | 3000               | 1000                  |
| 2019-01  | US      | 1           | 1              | 2000               | 2000                  |
| 2019-01  | DE      | 1           | 1              | 2000               | 2000                  |
+----------+---------+-------------+----------------+--------------------+-----------------------+
````
### Problem 19 Solution and Explanation:

```sql
SELECT month, 
       country, 
       COUNT(month) AS trans_count, 
       SUM(CASE WHEN state='approved' THEN 1 ELSE 0 END) AS approved_count, 
       SUM(amount) AS trans_total_amount, 
       SUM(CASE WHEN state='approved' THEN amount ELSE 0 END) AS approved_total_amount
FROM(
    SELECT *,
        CONCAT(CAST(YEAR(trans_date) AS CHAR), '-', CAST(LPAD(MONTH(trans_date), 2, '0') AS CHAR)) AS month
    FROM Transactions
) AS t1
GROUP BY 1,2;
```

**Let's break down this SQL query:**

- **`SELECT month, country, COUNT(month) AS trans_count, ... :`**
  - This part of the query selects the `month`, `country`, counts the number of transactions (`trans_count`), counts the number of approved transactions (`approved_count`), sums the total transaction amount (`trans_total_amount`), and sums the total amount of approved transactions (`approved_total_amount`).

- **`FROM(... ) AS t1:`**
  - This is a subquery that selects all columns from the Transactions table and adds a calculated column `month`. The `month` column is created by concatenating the year and zero-padded month of the `trans_date` using the `CONCAT` and `LPAD` functions.

- **`GROUP BY 1,2;`**
  - Groups the results by the first and second selected columns (`month` and `country`). The aggregate functions then operate on each unique combination of month and country.

**In summary,** this SQL query retrieves information about transactions, grouping them by month and country. It provides counts for the total number of transactions (`trans_count`) and the number of approved transactions (`approved_count`). It also calculates the total transaction amount (`trans_total_amount`) and the total amount of approved transactions (`approved_total_amount`) for each month and country combination.

---

### Problem 20: Immediate Food Delivery II

````
Table: Delivery
+-----------------------------+---------+
| Column Name                 | Type    |
+-----------------------------+---------+
| delivery_id                 | int     |
| customer_id                 | int     |
| order_date                  | date    |
| customer_pref_delivery_date | date    |
+-----------------------------+---------+
delivery_id is the column of unique values of this table.
The table holds information about food delivery to customers that make orders at some date and specify a preferred delivery date (on the same order date or after it).
 

If the customer's preferred delivery date is the same as the order date, then the order is called immediate; otherwise, it is called scheduled.

The first order of a customer is the order with the earliest order date that the customer made. It is guaranteed that a customer has precisely one first order.

Write a solution to find the percentage of immediate orders in the first orders of all customers, rounded to 2 decimal places.

The result format is in the following example.

Example 1:

Input: 
Delivery table:
+-------------+-------------+------------+-----------------------------+
| delivery_id | customer_id | order_date | customer_pref_delivery_date |
+-------------+-------------+------------+-----------------------------+
| 1           | 1           | 2019-08-01 | 2019-08-02                  |
| 2           | 2           | 2019-08-02 | 2019-08-02                  |
| 3           | 1           | 2019-08-11 | 2019-08-12                  |
| 4           | 3           | 2019-08-24 | 2019-08-24                  |
| 5           | 3           | 2019-08-21 | 2019-08-22                  |
| 6           | 2           | 2019-08-11 | 2019-08-13                  |
| 7           | 4           | 2019-08-09 | 2019-08-09                  |
+-------------+-------------+------------+-----------------------------+
Output: 
+----------------------+
| immediate_percentage |
+----------------------+
| 50.00                |
+----------------------+
Explanation: 
The customer id 1 has a first order with delivery id 1 and it is scheduled.
The customer id 2 has a first order with delivery id 2 and it is immediate.
The customer id 3 has a first order with delivery id 5 and it is scheduled.
The customer id 4 has a first order with delivery id 7 and it is immediate.
Hence, half the customers have immediate first orders.
````
### Problem Solution and Explanation:

```sql
SELECT
    ROUND(AVG(order_date = customer_pref_delivery_date) * 100, 2) AS immediate_percentage
FROM Delivery
WHERE (customer_id, order_date) IN (
  SELECT customer_id, MIN(order_date) 
  FROM Delivery
  GROUP BY customer_id
);
```

**Let's break down this SQL query:**

- **`SELECT ROUND(AVG(order_date = customer_pref_delivery_date) * 100, 2) AS immediate_percentage:`**
  - This part of the query calculates the percentage of orders where `order_date` matches `customer_pref_delivery_date` and rounds it to two decimal places.

- **`FROM Delivery:`**
  - Specifies the main table from which we want to retrieve data, and in this case, it's the Delivery table.

- **`WHERE (customer_id, order_date) IN (SELECT customer_id, MIN(order_date) FROM Delivery GROUP BY customer_id);`**
  - This condition filters the rows based on a subquery that selects the earliest (`MIN`) `order_date` for each `customer_id`. It uses a composite condition (`(customer_id, order_date)`) to ensure that both `customer_id` and `order_date` match the earliest order for each customer.

**In summary,** this SQL query calculates the percentage of orders where the `order_date` matches the `customer_pref_delivery_date` for each customer. It filters the rows to include only the earliest order for each customer and then calculates the average percentage across all customers, rounding the result to two decimal places.

----

# Hello, fellow learners! 🎉 
I'm excited to share my solutions and insights with you. I hope you find them valuable on your own learning path. 

Feel free to reach out if you have any questions or want to discuss any of the solutions. Let's embark on this journey of knowledge together! 📚💡

---
