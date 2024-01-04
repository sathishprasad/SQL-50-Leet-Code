# Day 2 of a 10-Day Learning Challenge 🚀

### Problem 6: Replace Employee ID with Unique Identifier
````
Table: Employees
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| name          | varchar |
+---------------+---------+
id is the primary key (column with unique values) for this table.
Each row of this table contains the id and the name of an employee in a company.
 

Table: EmployeeUNI
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| unique_id     | int     |
+---------------+---------+
(id, unique_id) is the primary key (combination of columns with unique values) for this table.
Each row of this table contains the id and the corresponding unique id of an employee in the company.
 

Write a solution to show the unique ID of each user, If a user does not have a unique ID replace just show null.

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: 
Employees table:
+----+----------+
| id | name     |
+----+----------+
| 1  | Alice    |
| 7  | Bob      |
| 11 | Meir     |
| 90 | Winston  |
| 3  | Jonathan |
+----+----------+
EmployeeUNI table:
+----+-----------+
| id | unique_id |
+----+-----------+
| 3  | 1         |
| 11 | 2         |
| 90 | 3         |
+----+-----------+
Output: 
+-----------+----------+
| unique_id | name     |
+-----------+----------+
| null      | Alice    |
| null      | Bob      |
| 2         | Meir     |
| 3         | Winston  |
| 1         | Jonathan |
+-----------+----------+
Explanation: 
Alice and Bob do not have a unique ID, We will show null instead.
The unique ID of Meir is 2.
The unique ID of Winston is 3.
The unique ID of Jonathan is 1.
````

### Problem 6 Solution and Explanation:

```sql
SELECT EmployeeUNI.unique_id, Employees.name
FROM Employees
LEFT JOIN EmployeeUNI
ON Employees.id = EmployeeUNI.id;
```

**Let's break down this SQL query:**

1. **SELECT EmployeeUNI.unique_id, Employees.name:**
   - This part of the query indicates the columns we want to retrieve in the result set: `EmployeeUNI.unique_id` and `Employees.name`.

2. **FROM Employees:**
   - Specifies the main table from which we want to retrieve data, and in this case, it's the `Employees` table.

3. **LEFT JOIN EmployeeUNI ON Employees.id = EmployeeUNI.id:**
   - This is a LEFT JOIN clause that combines data from the `Employees` table with the `EmployeeUNI` table based on the condition specified after the `ON` keyword.
   - `LEFT JOIN` ensures that all rows from the `Employees` table are included in the result set, even if there is no match in the `EmployeeUNI` table.

4. **ON Employees.id = EmployeeUNI.id:**
   - This part specifies the condition for the join, indicating that we want to match rows where the `id` column in the `Employees` table is equal to the `id` column in the `EmployeeUNI` table.

In summary, this SQL query retrieves the `unique_id` from the `EmployeeUNI` table and the `name` from the `Employees` table, linking them by the `id` column. The `LEFT JOIN` ensures that all employees from the `Employees` table are included, and if there's a match in the `EmployeeUNI` table, the corresponding `unique_id` is included; otherwise, it contains NULL values.

---

### Problem 7: Product Sales Analysis I

````
Table: Sales
+-------------+-------+
| Column Name | Type  |
+-------------+-------+
| sale_id     | int   |
| product_id  | int   |
| year        | int   |
| quantity    | int   |
| price       | int   |
+-------------+-------+
(sale_id, year) is the primary key (combination of columns with unique values) of this table.
product_id is a foreign key (reference column) to Product table.
Each row of this table shows a sale on the product product_id in a certain year.
Note that the price is per unit.
 

Table: Product
+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| product_id   | int     |
| product_name | varchar |
+--------------+---------+
product_id is the primary key (column with unique values) of this table.
Each row of this table indicates the product name of each product.
 

Write a solution to report the product_name, year, and price for each sale_id in the Sales table.

Return the resulting table in any order.

The result format is in the following example.


Example 1:

Input: 
Sales table:
+---------+------------+------+----------+-------+
| sale_id | product_id | year | quantity | price |
+---------+------------+------+----------+-------+ 
| 1       | 100        | 2008 | 10       | 5000  |
| 2       | 100        | 2009 | 12       | 5000  |
| 7       | 200        | 2011 | 15       | 9000  |
+---------+------------+------+----------+-------+
Product table:
+------------+--------------+
| product_id | product_name |
+------------+--------------+
| 100        | Nokia        |
| 200        | Apple        |
| 300        | Samsung      |
+------------+--------------+
Output: 
+--------------+-------+-------+
| product_name | year  | price |
+--------------+-------+-------+
| Nokia        | 2008  | 5000  |
| Nokia        | 2009  | 5000  |
| Apple        | 2011  | 9000  |
+--------------+-------+-------+
Explanation: 
From sale_id = 1, we can conclude that Nokia was sold for 5000 in the year 2008.
From sale_id = 2, we can conclude that Nokia was sold for 5000 in the year 2009.
From sale_id = 7, we can conclude that Apple was sold for 9000 in the year 2011.
````
### Problem 7 Solution and Explanation:
```sql
SELECT p.product_name, s.year, s.price
FROM Sales as s
INNER JOIN Product as p
ON s.product_id = p.product_id;
```

**Let's break down this SQL query:**

1. **SELECT p.product_name, s.year, s.price:**
   - This part of the query indicates the columns we want to retrieve in the result set: `p.product_name`, `s.year`, and `s.price`.

2. **FROM Sales as s:**
   - Specifies the main table from which we want to retrieve data, and in this case, it's the `Sales` table. It is given the alias `s` for brevity.

3. **INNER JOIN Product as p ON s.product_id = p.product_id:**
   - This is an INNER JOIN clause that combines data from the `Sales` table (aliased as `s`) with the `Product` table (aliased as `p`) based on the condition specified after the `ON` keyword.
   - It joins rows where the `product_id` column in the `Sales` table matches the `product_id` column in the `Product` table.

4. **ON s.product_id = p.product_id:**
   - This part specifies the condition for the join, indicating that we want to match rows where the `product_id` column in the `Sales` table (aliased as `s`) is equal to the `product_id` column in the `Product` table (aliased as `p`).

In summary, this SQL query retrieves the `product_name` from the `Product` table and the `year` and `price` from the `Sales` table, joining them based on the common `product_id` column. The result set will contain information about sales, including the product name associated with each sale. The use of INNER JOIN ensures that only matching rows from both tables are included in the result.

---

### Problem 8: Customer Who Visited but Did Not Make Any Transactions

````
Table: Visits
+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| visit_id    | int     |
| customer_id | int     |
+-------------+---------+
visit_id is the column with unique values for this table.
This table contains information about the customers who visited the mall.
 

Table: Transactions
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| transaction_id | int     |
| visit_id       | int     |
| amount         | int     |
+----------------+---------+
transaction_id is column with unique values for this table.
This table contains information about the transactions made during the visit_id.
 

Write a solution to find the IDs of the users who visited without making any transactions and the number of times they made these types of visits.

Return the result table sorted in any order.

The result format is in the following example.



Example 1:

Input: 
Visits
+----------+-------------+
| visit_id | customer_id |
+----------+-------------+
| 1        | 23          |
| 2        | 9           |
| 4        | 30          |
| 5        | 54          |
| 6        | 96          |
| 7        | 54          |
| 8        | 54          |
+----------+-------------+
Transactions
+----------------+----------+--------+
| transaction_id | visit_id | amount |
+----------------+----------+--------+
| 2              | 5        | 310    |
| 3              | 5        | 300    |
| 9              | 5        | 200    |
| 12             | 1        | 910    |
| 13             | 2        | 970    |
+----------------+----------+--------+
Output: 
+-------------+----------------+
| customer_id | count_no_trans |
+-------------+----------------+
| 54          | 2              |
| 30          | 1              |
| 96          | 1              |
+-------------+----------------+
Explanation: 
Customer with id = 23 visited the mall once and made one transaction during the visit with id = 12.
Customer with id = 9 visited the mall once and made one transaction during the visit with id = 13.
Customer with id = 30 visited the mall once and did not make any transactions.
Customer with id = 54 visited the mall three times. During 2 visits they did not make any transactions, and during one visit they made 3 transactions.
Customer with id = 96 visited the mall once and did not make any transactions.
As we can see, users with IDs 30 and 96 visited the mall one time without making any transactions. Also, user 54 visited the mall twice and did not make any transactions.
````
### Problem 8 Solution and Explanation:

```sql
SELECT customer_id, 
       COUNT(customer_id) as count_no_trans
FROM Visits
WHERE visit_id NOT IN (
    SELECT DISTINCT(v.visit_id)
    FROM Transactions as t
    INNER JOIN Visits as v
    ON t.visit_id = v.visit_id
)
GROUP BY customer_id
ORDER BY 2, 1;
```

**Let's break down this SQL query:**

1. **SELECT customer_id, COUNT(customer_id) as count_no_trans:**
   - This part of the query indicates the columns we want to retrieve in the result set: `customer_id` and the count of occurrences of each `customer_id` as `count_no_trans`.

2. **FROM Visits:**
   - Specifies the main table from which we want to retrieve data, and in this case, it's the `Visits` table.

3. **WHERE visit_id NOT IN (SELECT DISTINCT(v.visit_id) FROM Transactions as t INNER JOIN Visits as v ON t.visit_id = v.visit_id):**
   - This is a subquery that filters out rows from the main query. It selects distinct `visit_id` values from the `Transactions` table (aliased as `t`) and `Visits` table (aliased as `v`) where there is a match on the `visit_id` column.
   - The main query then filters out rows from the `Visits` table where the `visit_id` is not present in the result of the subquery.

4. **GROUP BY customer_id:**
   - This clause groups the result set by the `customer_id` column. The COUNT function is applied to each group, counting the number of occurrences of each `customer_id`.

5. **ORDER BY 2, 1:**
   - Orders the result set first by the second column (`count_no_trans`) in ascending order (`ORDER BY 2`) and then by the first column (`customer_id`) in ascending order (`ORDER BY 1`).

In summary, this SQL query retrieves the count of visits (`count_no_trans`) for each `customer_id` from the `Visits` table, excluding visits associated with transactions. The result is grouped by `customer_id` and ordered first by the count of visits and then by `customer_id`.

---

### Problem 9: Rising Temperature
````
Table: Weather
+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| id            | int     |
| recordDate    | date    |
| temperature   | int     |
+---------------+---------+
id is the column with unique values for this table.
This table contains information about the temperature on a certain day.
 

Write a solution to find all dates' Id with higher temperatures compared to its previous dates (yesterday).

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: 
Weather table:
+----+------------+-------------+
| id | recordDate | temperature |
+----+------------+-------------+
| 1  | 2015-01-01 | 10          |
| 2  | 2015-01-02 | 25          |
| 3  | 2015-01-03 | 20          |
| 4  | 2015-01-04 | 30          |
+----+------------+-------------+
Output: 
+----+
| id |
+----+
| 2  |
| 4  |
+----+
Explanation: 
In 2015-01-02, the temperature was higher than the previous day (10 -> 25).
In 2015-01-04, the temperature was higher than the previous day (20 -> 30).
````
### Problem 9 Solution and Explanation

```sql
SELECT a.id
FROM Weather a
INNER JOIN Weather b
ON a.recordDate = DATE_ADD(b.recordDate, INTERVAL 1 DAY)
WHERE a.temperature > b.temperature;
```

**Let's break down this SQL query:**

1. **SELECT a.id:**
   - This part of the query indicates the column we want to retrieve in the result set: `a.id`.

2. **FROM Weather a:**
   - Specifies the main table from which we want to retrieve data, and in this case, it's the `Weather` table. It is given the alias `a` for brevity.

3. **INNER JOIN Weather b ON a.recordDate = DATE_ADD(b.recordDate, INTERVAL 1 DAY):**
   - This is an INNER JOIN clause that combines data from the `Weather` table (aliased as `a`) with itself (aliased as `b`). It is matching rows where the `recordDate` in `a` is equal to the `recordDate` in `b` incremented by one day (`DATE_ADD(b.recordDate, INTERVAL 1 DAY)`).

4. **WHERE a.temperature > b.temperature:**
   - This part is a condition that filters the joined rows. It includes only those where the temperature in `a` is greater than the temperature in `b`.

In summary, this SQL query retrieves the `id` from the `Weather` table for rows where the temperature on a specific day (`a.recordDate`) is greater than the temperature on the previous day (`b.recordDate + 1 day`). The query is using a self-join to compare weather data for consecutive days.

---

### Problem 10: Average Time of Process per Machine
````
Table: Activity
+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| machine_id     | int     |
| process_id     | int     |
| activity_type  | enum    |
| timestamp      | float   |
+----------------+---------+
The table shows the user activities for a factory website.
(machine_id, process_id, activity_type) is the primary key (combination of columns with unique values) of this table.
machine_id is the ID of a machine.
process_id is the ID of a process running on the machine with ID machine_id.
activity_type is an ENUM (category) of type ('start', 'end').
timestamp is a float representing the current time in seconds.
'start' means the machine starts the process at the given timestamp and 'end' means the machine ends the process at the given timestamp.
The 'start' timestamp will always be before the 'end' timestamp for every (machine_id, process_id) pair.
 

There is a factory website that has several machines each running the same number of processes. Write a solution to find the average time each machine takes to complete a process.

The time to complete a process is the 'end' timestamp minus the 'start' timestamp. The average time is calculated by the total time to complete every process on the machine divided by the number of processes that were run.

The resulting table should have the machine_id along with the average time as processing_time, which should be rounded to 3 decimal places.

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: 
Activity table:
+------------+------------+---------------+-----------+
| machine_id | process_id | activity_type | timestamp |
+------------+------------+---------------+-----------+
| 0          | 0          | start         | 0.712     |
| 0          | 0          | end           | 1.520     |
| 0          | 1          | start         | 3.140     |
| 0          | 1          | end           | 4.120     |
| 1          | 0          | start         | 0.550     |
| 1          | 0          | end           | 1.550     |
| 1          | 1          | start         | 0.430     |
| 1          | 1          | end           | 1.420     |
| 2          | 0          | start         | 4.100     |
| 2          | 0          | end           | 4.512     |
| 2          | 1          | start         | 2.500     |
| 2          | 1          | end           | 5.000     |
+------------+------------+---------------+-----------+
Output: 
+------------+-----------------+
| machine_id | processing_time |
+------------+-----------------+
| 0          | 0.894           |
| 1          | 0.995           |
| 2          | 1.456           |
+------------+-----------------+
Explanation: 
There are 3 machines running 2 processes each.
Machine 0's average time is ((1.520 - 0.712) + (4.120 - 3.140)) / 2 = 0.894
Machine 1's average time is ((1.550 - 0.550) + (1.420 - 0.430)) / 2 = 0.995
Machine 2's average time is ((4.512 - 4.100) + (5.000 - 2.500)) / 2 = 1.456
````
### Problem 10 Solution and Explanation:

```sql
SELECT 
    start.machine_id,
    ROUND(AVG(end.timestamp - start.timestamp), 3) AS processing_time
FROM Activity AS start
INNER JOIN  Activity AS end
ON 
    start.machine_id = end.machine_id
    AND start.process_id = end.process_id
    AND start.activity_type = 'start'
    AND end.activity_type = 'end'
GROUP BY 
    start.machine_id;
```

**Let's break down this SQL query:**

1. **SELECT start.machine_id, ROUND(AVG(end.timestamp - start.timestamp), 3) AS processing_time:**
   - This part of the query indicates the columns we want to retrieve in the result set. It calculates the average processing time for each `machine_id` by subtracting the `start.timestamp` from the `end.timestamp` and rounding the result to three decimal places.

2. **FROM Activity AS start:**
   - Specifies the main table from which we want to retrieve data, and in this case, it's the `Activity` table. It is given the alias `start` for brevity.

3. **INNER JOIN Activity AS end ON start.machine_id = end.machine_id AND start.process_id = end.process_id AND start.activity_type = 'start' AND end.activity_type = 'end':**
   - This is an INNER JOIN clause that combines data from the `Activity` table (aliased as `start`) with itself (aliased as `end`). It matches rows where the `machine_id`, `process_id`, and `activity_type` are the same in both `start` and `end` activities, indicating the start and end of the same process on the same machine.

4. **GROUP BY start.machine_id:**
   - This clause groups the result set by the `machine_id` column. The AVG function is applied to the time differences for each group, calculating the average processing time.

In summary, this SQL query retrieves the average processing time (`processing_time`) for each machine (`machine_id`) by comparing the start and end activities in the `Activity` table. The result is grouped by `machine_id`.

---

# Hello, fellow learners! 🎉 
I'm excited to share my solutions and insights with you. I hope you find them valuable on your own learning path. 

Feel free to reach out if you have any questions or want to discuss any of the solutions. Let's embark on this journey of knowledge together! 📚💡

---
