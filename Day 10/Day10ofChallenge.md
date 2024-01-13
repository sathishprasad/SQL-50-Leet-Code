# Day 10 of a 10-Day Learning Challenge 🚀

### Problem 46: Second Highest Salary

```` 
Table: Employee

+-------------+------+
| Column Name | Type |
+-------------+------+
| id          | int  |
| salary      | int  |
+-------------+------+
id is the primary key (column with unique values) for this table.
Each row of this table contains information about the salary of an employee.
 

Write a solution to find the second highest salary from the Employee table. If there is no second highest salary, return null (return None in Pandas).

The result format is in the following example.

 

Example 1:

Input: 
Employee table:
+----+--------+
| id | salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
Output: 
+---------------------+
| SecondHighestSalary |
+---------------------+
| 200                 |
+---------------------+
Example 2:

Input: 
Employee table:
+----+--------+
| id | salary |
+----+--------+
| 1  | 100    |
+----+--------+
Output: 
+---------------------+
| SecondHighestSalary |
+---------------------+
| null                |
+---------------------+
````
### Problem 46 Solution and Explanation:

```sql
SELECT
    (SELECT DISTINCT Salary 
     FROM Employee 
     ORDER BY Salary DESC 
     LIMIT 1 OFFSET 1) 
AS SecondHighestSalary;
```

**Solution:**

This SQL query retrieves the second-highest salary from the `Employee` table. It uses a subquery within the `SELECT` statement to achieve this.

- **`SELECT DISTINCT Salary FROM Employee ORDER BY Salary DESC LIMIT 1 OFFSET 1:`**
  - The subquery selects distinct salaries (`DISTINCT Salary`) from the `Employee` table.
  - It orders the salaries in descending order (`ORDER BY Salary DESC`).
  - It limits the result set to one row (`LIMIT 1`).
  - It skips the first row, effectively obtaining the second-highest salary (`OFFSET 1`).

- **`AS SecondHighestSalary;`**
  - The result of the subquery is aliased as `SecondHighestSalary`.

**Explanation:**

The subquery is responsible for obtaining the second-highest salary from the `Employee` table. Here's a breakdown of the subquery:

- `DISTINCT Salary`: Ensures that only distinct salary values are considered.
- `ORDER BY Salary DESC`: Orders the salary values in descending order, placing the highest salary first.
- `LIMIT 1 OFFSET 1`: Limits the result set to one row and skips the first row, effectively retrieving the second-highest salary.

The outer `SELECT` statement aliases the result of the subquery as `SecondHighestSalary`.

In summary, this SQL query provides a solution to retrieve the second-highest salary from the `Employee` table, and the result is presented as `SecondHighestSalary`.

---

### Problem 47:  Group Sold Products By The Date

````
Table Activities:

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| sell_date   | date    |
| product     | varchar |
+-------------+---------+
There is no primary key (column with unique values) for this table. It may contain duplicates.
Each row of this table contains the product name and the date it was sold in a market.
 

Write a solution to find for each date the number of different products sold and their names.

The sold products names for each date should be sorted lexicographically.

Return the result table ordered by sell_date.

The result format is in the following example.

 

Example 1:

Input: 
Activities table:
+------------+------------+
| sell_date  | product     |
+------------+------------+
| 2020-05-30 | Headphone  |
| 2020-06-01 | Pencil     |
| 2020-06-02 | Mask       |
| 2020-05-30 | Basketball |
| 2020-06-01 | Bible      |
| 2020-06-02 | Mask       |
| 2020-05-30 | T-Shirt    |
+------------+------------+
Output: 
+------------+----------+------------------------------+
| sell_date  | num_sold | products                     |
+------------+----------+------------------------------+
| 2020-05-30 | 3        | Basketball,Headphone,T-shirt |
| 2020-06-01 | 2        | Bible,Pencil                 |
| 2020-06-02 | 1        | Mask                         |
+------------+----------+------------------------------+
Explanation: 
For 2020-05-30, Sold items were (Headphone, Basketball, T-shirt), we sort them lexicographically and separate them by a comma.
For 2020-06-01, Sold items were (Pencil, Bible), we sort them lexicographically and separate them by a comma.
For 2020-06-02, the Sold item is (Mask), we just return it.

````

### Problem 47 Solution and Explanation:

```sql
SELECT 
    sell_date, 
    COUNT(DISTINCT product) AS num_sold,
    GROUP_CONCAT(DISTINCT product ORDER BY product ASC SEPARATOR ',') AS products
FROM Activities 
GROUP BY sell_date 
ORDER BY sell_date ASC;
```

**Solution:**

This SQL query provides information about the number of distinct products sold (`num_sold`) and the list of distinct products (`products`) for each `sell_date` in the `Activities` table. It utilizes aggregate functions and grouping.

- **`SELECT sell_date, COUNT(DISTINCT product) AS num_sold, ...:`**
  - Selects the `sell_date` column.
  - Counts the number of distinct products for each `sell_date` using `COUNT(DISTINCT product)`.
  - Uses the `GROUP_CONCAT` function to concatenate distinct product names into a comma-separated list.

- **`GROUP BY sell_date:`**
  - Groups the results based on the `sell_date` column.

- **`ORDER BY sell_date ASC;`**
  - Orders the result set by `sell_date` in ascending order.

**Explanation:**

- **`sell_date`:** Represents the date when the sales activities occurred.

- **`COUNT(DISTINCT product) AS num_sold`:** Calculates the number of distinct products sold for each `sell_date`.

- **`GROUP_CONCAT(DISTINCT product ORDER BY product ASC SEPARATOR ',') AS products`:** Concatenates distinct product names into a comma-separated list (`products`) for each `sell_date`. The `ORDER BY product ASC` ensures the products are ordered alphabetically in the concatenated list.

- **`GROUP BY sell_date`:** Groups the results based on the `sell_date` column, so calculations are performed for each unique `sell_date`.

- **`ORDER BY sell_date ASC;`** Orders the final result set by `sell_date` in ascending order.

In summary, this SQL query provides a solution to obtain the number of distinct products sold and a comma-separated list of distinct products for each `sell_date` in the `Activities` table. The result set includes columns `sell_date`, `num_sold`, and `products`.

---

### Problem 48: List the Products Ordered in a Period

````
Table: Products

+------------------+---------+
| Column Name      | Type    |
+------------------+---------+
| product_id       | int     |
| product_name     | varchar |
| product_category | varchar |
+------------------+---------+
product_id is the primary key (column with unique values) for this table.
This table contains data about the company's products.
 

Table: Orders

+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| product_id    | int     |
| order_date    | date    |
| unit          | int     |
+---------------+---------+
This table may have duplicate rows.
product_id is a foreign key (reference column) to the Products table.
unit is the number of products ordered in order_date.
 

Write a solution to get the names of products that have at least 100 units ordered in February 2020 and their amount.

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: 
Products table:
+-------------+-----------------------+------------------+
| product_id  | product_name          | product_category |
+-------------+-----------------------+------------------+
| 1           | Leetcode Solutions    | Book             |
| 2           | Jewels of Stringology | Book             |
| 3           | HP                    | Laptop           |
| 4           | Lenovo                | Laptop           |
| 5           | Leetcode Kit          | T-shirt          |
+-------------+-----------------------+------------------+
Orders table:
+--------------+--------------+----------+
| product_id   | order_date   | unit     |
+--------------+--------------+----------+
| 1            | 2020-02-05   | 60       |
| 1            | 2020-02-10   | 70       |
| 2            | 2020-01-18   | 30       |
| 2            | 2020-02-11   | 80       |
| 3            | 2020-02-17   | 2        |
| 3            | 2020-02-24   | 3        |
| 4            | 2020-03-01   | 20       |
| 4            | 2020-03-04   | 30       |
| 4            | 2020-03-04   | 60       |
| 5            | 2020-02-25   | 50       |
| 5            | 2020-02-27   | 50       |
| 5            | 2020-03-01   | 50       |
+--------------+--------------+----------+
Output: 
+--------------------+---------+
| product_name       | unit    |
+--------------------+---------+
| Leetcode Solutions | 130     |
| Leetcode Kit       | 100     |
+--------------------+---------+
Explanation: 
Products with product_id = 1 is ordered in February a total of (60 + 70) = 130.
Products with product_id = 2 is ordered in February a total of 80.
Products with product_id = 3 is ordered in February a total of (2 + 3) = 5.
Products with product_id = 4 was not ordered in February 2020.
Products with product_id = 5 is ordered in February a total of (50 + 50) = 100.
````
### Problem 48 Solution and Explanation:

```sql
SELECT p.product_name,
       SUM(o.unit) AS unit
FROM Products p
LEFT JOIN Orders o 
ON p.product_id = o.product_id
WHERE o.order_date BETWEEN '2020-02-01' AND '2020-02-29'
GROUP BY p.product_id
HAVING unit >= 100;
```

**Solution:**

This SQL query retrieves information about products and their total units sold within a specific date range. It utilizes a `LEFT JOIN` between the `Products` and `Orders` tables, and applies filtering conditions.

- **`SELECT p.product_name, SUM(o.unit) AS unit FROM Products p ...:`**
  - Selects the `product_name` from the `Products` table.
  - Calculates the total units sold (`SUM(o.unit)`) for each product.

- **`LEFT JOIN Orders o ON p.product_id = o.product_id`:**
  - Performs a left join between the `Products` and `Orders` tables based on the `product_id` column.

- **`WHERE o.order_date BETWEEN '2020-02-01' AND '2020-02-29'`:**
  - Filters the joined result to include only orders placed within the specified date range.

- **`GROUP BY p.product_id`:**
  - Groups the results based on the `product_id` column.

- **`HAVING unit >= 100;`:**
  - Filters the grouped results to include only those where the total units (`unit`) sold are greater than or equal to 100.

**Explanation:**

- **`p.product_name`:** Represents the name of each product.

- **`SUM(o.unit) AS unit`:** Calculates the total units sold for each product.

- **`LEFT JOIN Orders o ON p.product_id = o.product_id`:** Performs a left join between the `Products` and `Orders` tables based on the `product_id`.

- **`WHERE o.order_date BETWEEN '2020-02-01' AND '2020-02-29'`:** Filters the joined result to include only orders placed within the specified date range.

- **`GROUP BY p.product_id`:** Groups the results based on the `product_id` to calculate aggregate functions for each product.

- **`HAVING unit >= 100;`:** Filters the grouped results to include only those products with total units sold greater than or equal to 100.

In summary, this SQL query provides a solution to retrieve product names and their total units sold within a specific date range, considering only products with total units sold of 100 or more. The result set includes columns `product_name` and `unit`.

---

### Problem 49: Find Users With Valid E-Mails

````
Table: Users

+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| user_id       | int     |
| name          | varchar |
| mail          | varchar |
+---------------+---------+
user_id is the primary key (column with unique values) for this table.
This table contains information of the users signed up in a website. Some e-mails are invalid.
 

Write a solution to find the users who have valid emails.

A valid e-mail has a prefix name and a domain where:

The prefix name is a string that may contain letters (upper or lower case), digits, underscore '_', period '.', and/or dash '-'. The prefix name must start with a letter.
The domain is '@leetcode.com'.
Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: 
Users table:
+---------+-----------+-------------------------+
| user_id | name      | mail                    |
+---------+-----------+-------------------------+
| 1       | Winston   | winston@leetcode.com    |
| 2       | Jonathan  | jonathanisgreat         |
| 3       | Annabelle | bella-@leetcode.com     |
| 4       | Sally     | sally.come@leetcode.com |
| 5       | Marwan    | quarz#2020@leetcode.com |
| 6       | David     | david69@gmail.com       |
| 7       | Shapiro   | .shapo@leetcode.com     |
+---------+-----------+-------------------------+
Output: 
+---------+-----------+-------------------------+
| user_id | name      | mail                    |
+---------+-----------+-------------------------+
| 1       | Winston   | winston@leetcode.com    |
| 3       | Annabelle | bella-@leetcode.com     |
| 4       | Sally     | sally.come@leetcode.com |
+---------+-----------+-------------------------+
Explanation: 
The mail of user 2 does not have a domain.
The mail of user 5 has the # sign which is not allowed.
The mail of user 6 does not have the leetcode domain.
The mail of user 7 starts with a period.
````

### Problem 45 Solution and Explanation:

```sql
SELECT *
FROM Users
WHERE mail REGEXP '^[a-zA-Z][a-zA-Z0-9_.-]*@[l]eetcode[.]com$';
```

**Solution:**

This SQL query retrieves records from the `Users` table where the `mail` column matches a specific pattern using a regular expression (`REGEXP`).

- **`SELECT * FROM Users:`**
  - Specifies that all columns (`*`) from the `Users` table will be included in the result set.

- **`WHERE mail REGEXP '^[a-zA-Z][a-zA-Z0-9_.-]*@[l]eetcode[.]com$';`:**
  - Applies a condition to filter rows based on the regular expression pattern.
  - `^[a-zA-Z]`: Ensures that the email starts with a letter (uppercase or lowercase).
  - `[a-zA-Z0-9_.-]*`: Allows letters, numbers, underscores, dots, and hyphens in the local part of the email (before the '@').
  - `@`: Matches the '@' symbol.
  - `[l]eetcode[.]com$`: Ensures that the domain is 'leetcode.com', with 'l' as the first letter.
  - `$`: Specifies the end of the string.

**Explanation:**

The query aims to retrieve rows from the `Users` table where the `mail` column follows a specific email pattern. Here's a breakdown of the regular expression pattern:

- `^[a-zA-Z]`: Specifies that the email must start with a letter (uppercase or lowercase).
- `[a-zA-Z0-9_.-]*`: Allows for any combination of letters, numbers, underscores, dots, and hyphens in the local part of the email (before the '@').
- `@[l]eetcode[.]com$`: Ensures that the domain is 'leetcode.com' and starts with the letter 'l'. The `$` ensures that the pattern matches until the end of the string.

In summary, this SQL query provides a solution to retrieve records from the `Users` table where the email column follows a specific pattern, matching the conditions specified in the regular expression. The result set includes all columns from the `Users` table for rows that meet the specified criteria.

---

# Hello, fellow learners! 🎉 
I'm excited to share my solutions and insights with you. I hope you find them valuable on your own learning path. 

Feel free to reach out if you have any questions or want to discuss any of the solutions. Let's embark on this journey of knowledge together! 📚💡

---
