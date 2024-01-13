# Day 6 of a 10-Day Learning Challenge 🚀

### Problem 26: Find Followers Count

```` 
Table: Followers

+-------------+------+
| Column Name | Type |
+-------------+------+
| user_id     | int  |
| follower_id | int  |
+-------------+------+
(user_id, follower_id) is the primary key (combination of columns with unique values) for this table.
This table contains the IDs of a user and a follower in a social media app where the follower follows the user.
 

Write a solution that will, for each user, return the number of followers.

Return the result table ordered by user_id in ascending order.

The result format is in the following example.

 

Example 1:

Input: 
Followers table:
+---------+-------------+
| user_id | follower_id |
+---------+-------------+
| 0       | 1           |
| 1       | 0           |
| 2       | 0           |
| 2       | 1           |
+---------+-------------+
Output: 
+---------+----------------+
| user_id | followers_count|
+---------+----------------+
| 0       | 1              |
| 1       | 1              |
| 2       | 2              |
+---------+----------------+
Explanation: 
The followers of 0 are {1}
The followers of 1 are {0}
The followers of 2 are {0,1}
````
### Problem 26 Solution and Explanation:

```sql
SELECT user_id, COUNT(follower_id) AS followers_count
FROM Followers
GROUP BY 1
ORDER BY 1;
```

**Solution:**

This SQL query is designed to retrieve information about the count of followers for each user from the Followers table. The solution is achieved through the following key components:

- **`SELECT user_id, COUNT(follower_id) AS followers_count:`**
  - This part of the query specifies the columns `user_id` and the count of distinct `follower_id` for each user. The result is aliased as `followers_count`.

- **`FROM Followers:`**
  - Specifies the main table from which data is retrieved, and in this case, it's the Followers table.

- **`GROUP BY 1:`**
  - Groups the results by the first selected column (`user_id`).

- **`ORDER BY 1;`**
  - Orders the result set by the first selected column (`user_id`) in ascending order.

**Explanation:**

The query aims to analyze and retrieve information about the count of followers for each user from the Followers table. The `COUNT(follower_id)` calculates the number of followers for each user, and the `GROUP BY user_id` groups the results based on the unique user IDs.

The `ORDER BY 1` clause ensures that the result set is ordered by the `user_id` in ascending order.

In summary, this SQL query provides a solution to obtain the count of followers for each user from the Followers table. The result set includes columns `user_id` and `followers_count`, with rows representing each unique user and their corresponding count of followers.

----

### Problem 27: Biggest Single Number

````  
Table: MyNumbers

+-------------+------+
| Column Name | Type |
+-------------+------+
| num         | int  |
+-------------+------+
This table may contain duplicates (In other words, there is no primary key for this table in SQL).
Each row of this table contains an integer.
 

A single number is a number that appeared only once in the MyNumbers table.

Find the largest single number. If there is no single number, report null.

The result format is in the following example.

 

Example 1:

Input: 
MyNumbers table:
+-----+
| num |
+-----+
| 8   |
| 8   |
| 3   |
| 3   |
| 1   |
| 4   |
| 5   |
| 6   |
+-----+
Output: 
+-----+
| num |
+-----+
| 6   |
+-----+
Explanation: The single numbers are 1, 4, 5, and 6.
Since 6 is the largest single number, we return it.
Example 2:

Input: 
MyNumbers table:
+-----+
| num |
+-----+
| 8   |
| 8   |
| 7   |
| 7   |
| 3   |
| 3   |
| 3   |
+-----+
Output: 
+------+
| num  |
+------+
| null |
+------+
Explanation: There are no single numbers in the input table so we return null.
````
### Problem 27 Solution and Explanation:

```sql
SELECT MAX(num) AS num
FROM (
    SELECT num
    FROM MyNumbers
    GROUP BY 1
    HAVING COUNT(num) = 1
) AS t1;
```

**Solution:**

This SQL query is designed to retrieve the maximum (MAX) value among the numbers that occur only once in the MyNumbers table. The solution is achieved through the following key components:

- **`SELECT MAX(num) AS num:`**
  - This part of the query specifies the maximum value (`MAX(num)`) among the numbers that occur only once. The result is aliased as `num`.

- **`FROM (SELECT num FROM MyNumbers GROUP BY 1 HAVING COUNT(num) = 1) AS t1:`**
  - This subquery selects distinct numbers (`num`) from the MyNumbers table that occur only once. The `GROUP BY 1` groups the results by the first selected column (`num`), and the `HAVING COUNT(num) = 1` filters the results to include only those numbers that occur exactly once.

**Explanation:**

The query aims to identify and retrieve the maximum value among the numbers that occur only once in the MyNumbers table. The subquery is used to find distinct numbers with a count of 1, and then the outer query selects the maximum value among those numbers.

In summary, this SQL query provides a solution to find the maximum value among the numbers that occur only once in the MyNumbers table. The result set includes a single column `num` representing the maximum value meeting the specified condition.

---

### Problem 28: Customers Who Bought All Products

````  
Table: Customer

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| customer_id | int     |
| product_key | int     |
+-------------+---------+
This table may contain duplicates rows. 
customer_id is not NULL.
product_key is a foreign key (reference column) to Product table.
 

Table: Product

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| product_key | int     |
+-------------+---------+
product_key is the primary key (column with unique values) for this table.
 

Write a solution to report the customer ids from the Customer table that bought all the products in the Product table.

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: 
Customer table:
+-------------+-------------+
| customer_id | product_key |
+-------------+-------------+
| 1           | 5           |
| 2           | 6           |
| 3           | 5           |
| 3           | 6           |
| 1           | 6           |
+-------------+-------------+
Product table:
+-------------+
| product_key |
+-------------+
| 5           |
| 6           |
+-------------+
Output: 
+-------------+
| customer_id |
+-------------+
| 1           |
| 3           |
+-------------+
Explanation: 
The customers who bought all the products (5 and 6) are customers with IDs 1 and 3.
````
### Problem 28 Solution and Explanation:

```sql
SELECT customer_id
FROM Customer
GROUP BY customer_id
HAVING COUNT(DISTINCT product_key) = (SELECT COUNT(DISTINCT product_key) FROM Product);
```

**Solution:**

This SQL query is designed to retrieve customer IDs from the Customer table where each customer has purchased all distinct products available in the Product table. The solution is achieved through the following key components:

- **`SELECT customer_id:`**
  - This part of the query specifies the column `customer_id` that will be included in the result set.

- **`FROM Customer:`**
  - Specifies the main table from which data is retrieved, and in this case, it's the Customer table.

- **`GROUP BY customer_id:`**
  - Groups the results by the `customer_id` column.

- **`HAVING COUNT(DISTINCT product_key) = (SELECT COUNT(DISTINCT product_key) FROM Product):`**
  - Filters the grouped results to include only those customers who have purchased all distinct products. The `HAVING` clause checks if the count of distinct `product_key` for each customer is equal to the total count of distinct `product_key` in the Product table.

- **`SELECT COUNT(DISTINCT product_key) FROM Product:`**
  - This subquery calculates the total count of distinct `product_key` in the Product table.

**Explanation:**

The query aims to identify and retrieve customer IDs who have purchased all distinct products available in the Product table. The `COUNT(DISTINCT product_key)` ensures that each customer has purchased a unique set of products, and the `HAVING` clause compares this count with the total count of distinct products.

In summary, this SQL query provides a solution to find customer IDs that have purchased all distinct products available in the Product table. The result set includes the `customer_id` values for customers meeting the specified condition.

---

### Problem 29: The Number of Employees Which Report to Each Employee

````  
Table: Employees

+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| employee_id | int      |
| name        | varchar  |
| reports_to  | int      |
| age         | int      |
+-------------+----------+
employee_id is the column with unique values for this table.
This table contains information about the employees and the id of the manager they report to. Some employees do not report to anyone (reports_to is null). 
 

For this problem, we will consider a manager an employee who has at least 1 other employee reporting to them.

Write a solution to report the ids and the names of all managers, the number of employees who report directly to them, and the average age of the reports rounded to the nearest integer.

Return the result table ordered by employee_id.

The result format is in the following example.

 

Example 1:

Input: 
Employees table:
+-------------+---------+------------+-----+
| employee_id | name    | reports_to | age |
+-------------+---------+------------+-----+
| 9           | Hercy   | null       | 43  |
| 6           | Alice   | 9          | 41  |
| 4           | Bob     | 9          | 36  |
| 2           | Winston | null       | 37  |
+-------------+---------+------------+-----+
Output: 
+-------------+-------+---------------+-------------+
| employee_id | name  | reports_count | average_age |
+-------------+-------+---------------+-------------+
| 9           | Hercy | 2             | 39          |
+-------------+-------+---------------+-------------+
Explanation: Hercy has 2 people report directly to him, Alice and Bob. Their average age is (41+36)/2 = 38.5, which is 39 after rounding it to the nearest integer.
````
### Problem 29 Solution and Explanation:

```sql
SELECT employee_id,
       name, 
       COUNT(*) AS reports_count, 
       ROUND(AVG(age)) AS average_age
FROM (
    SELECT e1.employee_id, e1.name, e2.age
    FROM Employees AS e1
    INNER JOIN Employees AS e2
    ON e1.employee_id = e2.reports_to
) AS t1
GROUP BY 1
ORDER BY 1;
```

**Solution:**

This SQL query is designed to retrieve information about employees and their reports, including the count of reports (`reports_count`) and the average age (`average_age`) of the employees they manage. The solution is achieved through the following key components:

- **`SELECT employee_id, name, COUNT(*) AS reports_count, ROUND(AVG(age)) AS average_age:`**
  - This part of the query specifies the columns `employee_id`, `name`, `reports_count` (count of reports), and `average_age` (rounded average age).

- **`FROM (SELECT e1.employee_id, e1.name, e2.age ... ) AS t1:`**
  - This subquery selects relevant information from the Employees table, including the `employee_id`, `name`, and `age` of employees and their reports. It uses an `INNER JOIN` to match employees with their reports based on the `employee_id` and `reports_to` relationship.

- **`GROUP BY 1:`**
  - Groups the results by the first selected column (`employee_id`).

- **`ORDER BY 1;`**
  - Orders the result set by the first selected column (`employee_id`) in ascending order.

**Explanation:**

The query aims to provide insights into employees and their reports by calculating the count of reports (`reports_count`) and the average age (`average_age`) of the employees they manage. The subquery performs an inner join on the Employees table to establish the relationship between employees and their reports.

In summary, this SQL query offers a solution to analyze the reporting structure within the Employees table, providing information about the count of reports and the average age for each employee. The result set includes columns such as `employee_id`, `name`, `reports_count`, and `average_age`.

---
 ### Problem 30: Primary Department for Each Employee

````
Table: Employee

+---------------+---------+
| Column Name   |  Type   |
+---------------+---------+
| employee_id   | int     |
| department_id | int     |
| primary_flag  | varchar |
+---------------+---------+
(employee_id, department_id) is the primary key (combination of columns with unique values) for this table.
employee_id is the id of the employee.
department_id is the id of the department to which the employee belongs.
primary_flag is an ENUM (category) of type ('Y', 'N'). If the flag is 'Y', the department is the primary department for the employee. If the flag is 'N', the department is not the primary.
 

Employees can belong to multiple departments. When the employee joins other departments, they need to decide which department is their primary department. Note that when an employee belongs to only one department, their primary column is 'N'.

Write a solution to report all the employees with their primary department. For employees who belong to one department, report their only department.

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: 
Employee table:
+-------------+---------------+--------------+
| employee_id | department_id | primary_flag |
+-------------+---------------+--------------+
| 1           | 1             | N            |
| 2           | 1             | Y            |
| 2           | 2             | N            |
| 3           | 3             | N            |
| 4           | 2             | N            |
| 4           | 3             | Y            |
| 4           | 4             | N            |
+-------------+---------------+--------------+
Output: 
+-------------+---------------+
| employee_id | department_id |
+-------------+---------------+
| 1           | 1             |
| 2           | 1             |
| 3           | 3             |
| 4           | 3             |
+-------------+---------------+
Explanation: 
- The Primary department for employee 1 is 1.
- The Primary department for employee 2 is 1.
- The Primary department for employee 3 is 3.
- The Primary department for employee 4 is 3.
````
### Problem 30 Solution and Explanation:

```sql
SELECT employee_id,
       department_id
FROM Employee 
WHERE primary_flag = 'Y' 
UNION 
SELECT employee_id, 
       department_id  
FROM Employee 
GROUP BY employee_id 
HAVING COUNT(*) = 1;
```

**Solution:**

This SQL query is designed to retrieve information about employees and their respective department IDs based on two conditions: employees with `primary_flag` set to 'Y' and employees who belong to departments where they are the only member. The solution is achieved through the following key components:

- **`SELECT employee_id, department_id FROM Employee WHERE primary_flag = 'Y' UNION ...`**
  - This part of the query selects the `employee_id` and `department_id` for employees where `primary_flag` is set to 'Y'.

- **`SELECT employee_id, department_id FROM Employee GROUP BY employee_id HAVING COUNT(*) = 1;`**
  - This part of the query selects the `employee_id` and `department_id` for employees who belong to departments where they are the only member. The `GROUP BY` and `HAVING COUNT(*) = 1` ensure that only employees with a count of 1 (i.e., the only member of their department) are included.

- **`UNION:`**
  - The `UNION` operator combines the results of the two `SELECT` statements, removing duplicates and presenting a unified result set.

**Explanation:**

The query aims to provide a comprehensive list of employees and their department IDs based on two conditions. The first condition focuses on employees with `primary_flag` set to 'Y', and the second condition identifies employees who are the only members of their respective departments.

In summary, this SQL query offers a solution to gather information about employees and their department affiliations, considering both the primary flag and unique department membership. The result set includes columns such as `employee_id` and `department_id`.

---

# Hello, fellow learners! 🎉 
I'm excited to share my solutions and insights with you. I hope you find them valuable on your own learning path. 

Feel free to reach out if you have any questions or want to discuss any of the solutions. Let's embark on this journey of knowledge together! 📚💡

---


