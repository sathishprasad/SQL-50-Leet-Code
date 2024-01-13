# Day 7 of a 10-Day Learning Challenge 🚀

### Problem 31: Triangle Judgement

```` 
Table: Triangle

+-------------+------+
| Column Name | Type |
+-------------+------+
| x           | int  |
| y           | int  |
| z           | int  |
+-------------+------+
In SQL, (x, y, z) is the primary key column for this table.
Each row of this table contains the lengths of three line segments.
 

Report for every three line segments whether they can form a triangle.

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: 
Triangle table:
+----+----+----+
| x  | y  | z  |
+----+----+----+
| 13 | 15 | 30 |
| 10 | 20 | 15 |
+----+----+----+
Output: 
+----+----+----+----------+
| x  | y  | z  | triangle |
+----+----+----+----------+
| 13 | 15 | 30 | No       |
| 10 | 20 | 15 | Yes      |
+----+----+----+----------+
````
### Problem 31 Solution and Explanation:

```sql
SELECT x, y, z, 
       (CASE WHEN ABS(x - y) < z AND x + y > z THEN 'Yes' ELSE 'No' END) AS triangle
FROM Triangle;
```

**Solution:**

This SQL query is designed to analyze a table named Triangle, which presumably contains information about triangles represented by columns x, y, and z. The solution includes the following key components:

- **`SELECT x, y, z, ...:`**
  - This part of the query specifies the columns `x`, `y`, and `z` that will be included in the result set.

- **`(CASE WHEN ABS(x - y) < z AND x + y > z THEN 'Yes' ELSE 'No' END) AS triangle:`**
  - Utilizes a `CASE` statement to determine if the given set of values (x, y, z) represents a valid triangle. The conditions within the `CASE` statement check if the triangle inequality theorem is satisfied (`ABS(x - y) < z` and `x + y > z`). If both conditions are met, the result is 'Yes'; otherwise, it's 'No'. The result is aliased as `triangle`.

**Explanation:**

The query aims to evaluate whether the given values of x, y, and z can form a valid triangle. The conditions specified in the `CASE` statement are based on the triangle inequality theorem, which states that the sum of the lengths of any two sides of a triangle must be greater than the length of the third side.

- `ABS(x - y) < z`: Checks if the absolute difference between x and y is less than z.
- `x + y > z`: Checks if the sum of x and y is greater than z.

If both conditions are satisfied, the result is 'Yes,' indicating that the values can form a valid triangle. Otherwise, the result is 'No.'

In summary, this SQL query provides a solution to assess whether the values in the Triangle table represent valid triangles based on the triangle inequality theorem. The result set includes columns `x`, `y`, `z`, and a flag (`triangle`) indicating whether the values form a valid triangle ('Yes' or 'No').

---

### Problem 32: Consecutive Numbers

````  
Table: Logs

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| num         | varchar |
+-------------+---------+
In SQL, id is the primary key for this table.
id is an autoincrement column.
 

Find all numbers that appear at least three times consecutively.

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: 
Logs table:
+----+-----+
| id | num |
+----+-----+
| 1  | 1   |
| 2  | 1   |
| 3  | 1   |
| 4  | 2   |
| 5  | 1   |
| 6  | 2   |
| 7  | 2   |
+----+-----+
Output: 
+-----------------+
| ConsecutiveNums |
+-----------------+
| 1               |
+-----------------+
Explanation: 1 is the only number that appears consecutively for at least three times.
````
### Problem 32 Solution and Explanation:

```sql
SELECT DISTINCT(l1.num) AS ConsecutiveNums
FROM Logs AS l1
LEFT JOIN Logs l2
ON l1.id = l2.id + 1
LEFT JOIN Logs l3
ON l2.id = l3.id + 1
WHERE l1.num = l2.num AND l2.num = l3.num;
```

**Solution:**

This SQL query is designed to retrieve distinct numbers (`ConsecutiveNums`) from the Logs table where three consecutive rows have the same value in the 'num' column. The solution is achieved through the following key components:

- **`SELECT DISTINCT(l1.num) AS ConsecutiveNums:`**
  - This part of the query specifies the distinct numbers (`ConsecutiveNums`) that will be included in the result set.

- **`FROM Logs AS l1:`**
  - Specifies the main table from which data is retrieved, and in this case, it's the Logs table, aliased as `l1`.

- **`LEFT JOIN Logs l2 ON l1.id = l2.id + 1:`**
  - Performs a left join with the Logs table using the condition that the 'id' in l1 is equal to the 'id' in l2 plus 1.

- **`LEFT JOIN Logs l3 ON l2.id = l3.id + 1:`**
  - Performs another left join with the Logs table using the condition that the 'id' in l2 is equal to the 'id' in l3 plus 1.

- **`WHERE l1.num = l2.num AND l2.num = l3.num;`**
  - Filters the result set to include only those rows where the 'num' column has the same value in l1, l2, and l3, indicating three consecutive rows with the same number.

**Explanation:**

The query aims to identify distinct numbers in the Logs table where three consecutive rows have the same value in the 'num' column. The left joins with self-aliases (l1, l2, l3) help establish the relationships between consecutive rows.

In summary, this SQL query provides a solution to find distinct numbers that appear consecutively in three consecutive rows in the Logs table. The result set includes the distinct numbers as `ConsecutiveNums`.

---

### Problem 33: Product Price at a Given Date

````
Table: Products

+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| new_price     | int     |
| change_date   | date    |
+---------------+---------+
(product_id, change_date) is the primary key (combination of columns with unique values) of this table.
Each row of this table indicates that the price of some product was changed to a new price at some date.
 

Write a solution to find the prices of all products on 2019-08-16. Assume the price of all products before any change is 10.

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: 
Products table:
+------------+-----------+-------------+
| product_id | new_price | change_date |
+------------+-----------+-------------+
| 1          | 20        | 2019-08-14  |
| 2          | 50        | 2019-08-14  |
| 1          | 30        | 2019-08-15  |
| 1          | 35        | 2019-08-16  |
| 2          | 65        | 2019-08-17  |
| 3          | 20        | 2019-08-18  |
+------------+-----------+-------------+
Output: 
+------------+-------+
| product_id | price |
+------------+-------+
| 2          | 50    |
| 1          | 35    |
| 3          | 10    |
+------------+-------+  
````
### Problem 33 Solution and Explanation:

```sql
SELECT DISTINCT product_id, 10 AS price
FROM Products
WHERE product_id NOT IN (
    SELECT DISTINCT product_id
    FROM Products
    WHERE change_date <= '2019-08-16'
)

UNION

SELECT product_id, new_price AS price
FROM Products
WHERE (product_id, change_date) IN (
    SELECT product_id, MAX(change_date) AS date
    FROM Products
    WHERE change_date <= '2019-08-16'
    GROUP BY product_id
);
```

**Solution:**

This SQL query is designed to retrieve distinct product IDs and their corresponding prices from the Products table. The prices are determined based on the following conditions:

1. If a product's change date is after '2019-08-16,' set the price to 10.
2. If a product's change date is on or before '2019-08-16,' set the price to the new price associated with the latest change date for that product.

The solution is achieved through the following key components:

- **`SELECT DISTINCT product_id, 10 AS price:`**
  - This part of the query selects distinct product IDs and sets the price to 10 for products whose change date is after '2019-08-16.' The `NOT IN` clause filters out products that have change dates on or before '2019-08-16.'

- **`UNION:`**
  - The `UNION` operator combines the results of the two `SELECT` statements.

- **`SELECT product_id, new_price AS price:`**
  - This part of the query selects product IDs and their new prices for products whose change date is on or before '2019-08-16.' The `IN` clause ensures that only the latest change date for each product is considered.

**Explanation:**

The query aims to determine the prices for distinct products based on the specified conditions related to their change dates. The `UNION` of the two `SELECT` statements ensures that the final result set includes distinct product IDs with their corresponding prices.

- The first part handles products with change dates after '2019-08-16,' setting the price to 10.
- The second part handles products with change dates on or before '2019-08-16,' setting the price to the new price associated with the latest change date.

In summary, this SQL query provides a solution to determine the prices for distinct products based on the change dates in the Products table. The result set includes columns `product_id` and `price`.

----

### Problem 34: Last Person to Fit in the Bus

```` 
Table: Queue

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| person_id   | int     |
| person_name | varchar |
| weight      | int     |
| turn        | int     |
+-------------+---------+
person_id column contains unique values.
This table has the information about all people waiting for a bus.
The person_id and turn columns will contain all numbers from 1 to n, where n is the number of rows in the table.
turn determines the order of which the people will board the bus, where turn=1 denotes the first person to board and turn=n denotes the last person to board.
weight is the weight of the person in kilograms.
 

There is a queue of people waiting to board a bus. However, the bus has a weight limit of 1000 kilograms, so there may be some people who cannot board.

Write a solution to find the person_name of the last person that can fit on the bus without exceeding the weight limit. The test cases are generated such that the first person does not exceed the weight limit.

The result format is in the following example.

 

Example 1:

Input: 
Queue table:
+-----------+-------------+--------+------+
| person_id | person_name | weight | turn |
+-----------+-------------+--------+------+
| 5         | Alice       | 250    | 1    |
| 4         | Bob         | 175    | 5    |
| 3         | Alex        | 350    | 2    |
| 6         | John Cena   | 400    | 3    |
| 1         | Winston     | 500    | 6    |
| 2         | Marie       | 200    | 4    |
+-----------+-------------+--------+------+
Output: 
+-------------+
| person_name |
+-------------+
| John Cena   |
+-------------+
Explanation: The folowing table is ordered by the turn for simplicity.
+------+----+-----------+--------+--------------+
| Turn | ID | Name      | Weight | Total Weight |
+------+----+-----------+--------+--------------+
| 1    | 5  | Alice     | 250    | 250          |
| 2    | 3  | Alex      | 350    | 600          |
| 3    | 6  | John Cena | 400    | 1000         | (last person to board)
| 4    | 2  | Marie     | 200    | 1200         | (cannot board)
| 5    | 4  | Bob       | 175    | ___          |
| 6    | 1  | Winston   | 500    | ___          |
+------+----+-----------+--------+--------------+
````
### Problem 34 Solution and Explanation:

```sql
SELECT 
    q1.person_name
FROM Queue q1 JOIN Queue q2 ON q1.turn >= q2.turn
GROUP BY q1.turn
HAVING SUM(q2.weight) <= 1000
ORDER BY SUM(q2.weight) DESC
LIMIT 1;
```

**Solution:**

This SQL query is designed to identify the person with the earliest turn in the queue whose total weight, along with the weights of all people with equal or earlier turns, does not exceed 1000. The solution is achieved through the following key components:

- **`SELECT q1.person_name:`**
  - This part of the query specifies the column `person_name` from the Queue table that will be included in the result set.

- **`FROM Queue q1 JOIN Queue q2 ON q1.turn >= q2.turn:`**
  - Performs a self-join on the Queue table to compare the turns of different people in the queue. It joins the table with itself, pairing each person with those having equal or earlier turns.

- **`GROUP BY q1.turn:`**
  - Groups the results by the turn of each person in the queue.

- **`HAVING SUM(q2.weight) <= 1000:`**
  - Filters the grouped results to include only those groups (sets of people with the same or earlier turn) where the total weight does not exceed 1000.

- **`ORDER BY SUM(q2.weight) DESC:`**
  - Orders the result set in descending order based on the total weight of each group.

- **`LIMIT 1;`**
  - Limits the result set to only one row, representing the person with the earliest turn and a total weight not exceeding 1000.

**Explanation:**

The query aims to find the person with the earliest turn in the queue whose total weight, along with the weights of all people with equal or earlier turns, does not exceed 1000. The self-join is used to compare the turns, and the `HAVING` clause ensures that only groups meeting the weight condition are considered.

The `ORDER BY` clause arranges the result set in descending order based on the total weight, and `LIMIT 1` ensures that only the top result is selected.

In summary, this SQL query provides a solution to identify the person with the earliest turn in the queue meeting the specified weight condition. The result set includes the `person_name` of that person.

---

### Problem 35: Count Salary Categories

```` 
Table: Accounts
+-------------+------+
| Column Name | Type |
+-------------+------+
| account_id  | int  |
| income      | int  |
+-------------+------+
account_id is the primary key (column with unique values) for this table.
Each row contains information about the monthly income for one bank account.
 

Write a solution to calculate the number of bank accounts for each salary category. The salary categories are:

"Low Salary": All the salaries strictly less than $20000.
"Average Salary": All the salaries in the inclusive range [$20000, $50000].
"High Salary": All the salaries strictly greater than $50000.
The result table must contain all three categories. If there are no accounts in a category, return 0.

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: 
Accounts table:
+------------+--------+
| account_id | income |
+------------+--------+
| 3          | 108939 |
| 2          | 12747  |
| 8          | 87709  |
| 6          | 91796  |
+------------+--------+
Output: 
+----------------+----------------+
| category       | accounts_count |
+----------------+----------------+
| Low Salary     | 1              |
| Average Salary | 0              |
| High Salary    | 3              |
+----------------+----------------+
Explanation: 
Low Salary: Account 2.
Average Salary: No accounts.
High Salary: Accounts 3, 6, and 8.
````
### Problem 35 Solution and Explanation:

```sql
(SELECT 
    "Low Salary" AS category,
    (SELECT COUNT(*) FROM Accounts WHERE income < 20000) AS accounts_count)

UNION ALL

(SELECT 
    "Average Salary" AS category,
    (SELECT COUNT(*) FROM Accounts WHERE income >= 20000 AND income <= 50000) AS accounts_count)

UNION ALL

(SELECT 
    "High Salary" AS category,
    (SELECT COUNT(*) FROM Accounts WHERE income > 50000) AS accounts_count);
```

**Solution:**

This SQL query is designed to provide a summary of account categories based on income ranges. It uses three `UNION ALL` clauses to combine the results of three subqueries, each representing a different salary category. The solution includes the following key components:

- **`SELECT "Low Salary" AS category, ...`**
  - This part of the query represents the first salary category, where the category is set to "Low Salary," and the `accounts_count` is the count of accounts with income less than 20000.

- **`UNION ALL`**
  - The `UNION ALL` operator combines the results of the first subquery with the results of the subsequent subqueries.

- **`SELECT "Average Salary" AS category, ...`**
  - Represents the second salary category, where the category is set to "Average Salary," and the `accounts_count` is the count of accounts with income between 20000 and 50000 (inclusive).

- **`UNION ALL`**
  - Combines the results of the second subquery with the results from the previous subqueries.

- **`SELECT "High Salary" AS category, ...`**
  - Represents the third salary category, where the category is set to "High Salary," and the `accounts_count` is the count of accounts with income greater than 50000.

**Explanation:**

The query aims to provide a summary of account categories based on income ranges. Each subquery focuses on a specific salary category, and the `UNION ALL` operator combines the results into a single result set.

- The first subquery counts accounts with income less than 20000 for the "Low Salary" category.
- The second subquery counts accounts with income between 20000 and 50000 for the "Average Salary" category.
- The third subquery counts accounts with income greater than 50000 for the "High Salary" category.

In summary, this SQL query provides a solution to summarize account categories based on income ranges. The result set includes columns `category` and `accounts_count`.

---

# Hello, fellow learners! 🎉 
I'm excited to share my solutions and insights with you. I hope you find them valuable on your own learning path. 

Feel free to reach out if you have any questions or want to discuss any of the solutions. Let's embark on this journey of knowledge together! 📚💡

---

