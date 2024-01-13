# Day 9 of a 10-Day Learning Challenge 🚀

### Problem 41: Investments in 2016

````
Table: Insurance

+-------------+-------+
| Column Name | Type  |
+-------------+-------+
| pid         | int   |
| tiv_2015    | float |
| tiv_2016    | float |
| lat         | float |
| lon         | float |
+-------------+-------+
pid is the primary key (column with unique values) for this table.
Each row of this table contains information about one policy where:
pid is the policyholder's policy ID.
tiv_2015 is the total investment value in 2015 and tiv_2016 is the total investment value in 2016.
lat is the latitude of the policy holder's city. It's guaranteed that lat is not NULL.
lon is the longitude of the policy holder's city. It's guaranteed that lon is not NULL.
 

Write a solution to report the sum of all total investment values in 2016 tiv_2016, for all policyholders who:

have the same tiv_2015 value as one or more other policyholders, and
are not located in the same city as any other policyholder (i.e., the (lat, lon) attribute pairs must be unique).
Round tiv_2016 to two decimal places.

The result format is in the following example.

 

Example 1:

Input: 
Insurance table:
+-----+----------+----------+-----+-----+
| pid | tiv_2015 | tiv_2016 | lat | lon |
+-----+----------+----------+-----+-----+
| 1   | 10       | 5        | 10  | 10  |
| 2   | 20       | 20       | 20  | 20  |
| 3   | 10       | 30       | 20  | 20  |
| 4   | 10       | 40       | 40  | 40  |
+-----+----------+----------+-----+-----+
Output: 
+----------+
| tiv_2016 |
+----------+
| 45.00    |
+----------+
Explanation: 
The first record in the table, like the last record, meets both of the two criteria.
The tiv_2015 value 10 is the same as the third and fourth records, and its location is unique.

The second record does not meet any of the two criteria. Its tiv_2015 is not like any other policyholders and its location is the same as the third record, which makes the third record fail, too.
So, the result is the sum of tiv_2016 of the first and last record, which is 45.
````
```sql
SELECT ROUND(SUM(tiv_2016), 2) AS tiv_2016
FROM Insurance
WHERE tiv_2015 IN (
    SELECT tiv_2015
    FROM Insurance
    GROUP BY tiv_2015
    HAVING COUNT(*) > 1
)
AND (lat, lon) IN (
    SELECT lat, lon
    FROM Insurance
    GROUP BY lat, lon
    HAVING COUNT(*) = 1
);
```
### Problem 41 Solution and Explanation:

```sql
SELECT ROUND(SUM(tiv_2016), 2) AS tiv_2016
FROM Insurance
WHERE tiv_2015 IN (
    SELECT tiv_2015
    FROM Insurance
    GROUP BY tiv_2015
    HAVING COUNT(*) > 1
)
AND (lat, lon) IN (
    SELECT lat, lon
    FROM Insurance
    GROUP BY lat, lon
    HAVING COUNT(*) = 1
);
```

**Solution:**

This SQL query calculates the rounded sum of the `tiv_2016` column from the `Insurance` table. The calculation includes only rows that meet specific conditions related to the `tiv_2015` and geographical coordinates (`lat` and `lon`).

- The first condition (`tiv_2015 IN (...)`) checks for rows where the `tiv_2015` value is present in the result set of the subquery. The subquery identifies unique `tiv_2015` values that occur more than once in the `Insurance` table.

- The second condition (`(lat, lon) IN (...)`) checks for rows where the pair of geographical coordinates (`lat` and `lon`) is present in the result set of another subquery. The subquery identifies unique pairs of coordinates that occur exactly once in the `Insurance` table.

The main query then calculates the rounded sum of the `tiv_2016` column for the filtered rows.

**Explanation:**

The query aims to provide information about the total amount spent and the average amount spent by customers over a 7-day window for each visit date. The subqueries calculate these amounts within the specified date range, and the main query groups the results by the `visited_on` date.

- The first subquery calculates the total amount spent (`amount`) by customers in the 7-day window before each visit date.
- The second subquery calculates the average amount spent (`average_amount`) by customers over the same 7-day window.

The `ROUND` function is used to round the average amount to 2 decimal places.

In summary, this SQL query provides a solution to analyze customer spending patterns over a 7-day window for each visit date. The result set includes columns `visited_on`, `amount`, and `average_amount`.

---

### Problem 42: Department Top Three Salaries

````
Table: Employee

+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| id           | int     |
| name         | varchar |
| salary       | int     |
| departmentId | int     |
+--------------+---------+
id is the primary key (column with unique values) for this table.
departmentId is a foreign key (reference column) of the ID from the Department table.
Each row of this table indicates the ID, name, and salary of an employee. It also contains the ID of their department.
 

Table: Department

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
+-------------+---------+
id is the primary key (column with unique values) for this table.
Each row of this table indicates the ID of a department and its name.
 

A company's executives are interested in seeing who earns the most money in each of the company's departments. A high earner in a department is an employee who has a salary in the top three unique salaries for that department.

Write a solution to find the employees who are high earners in each of the departments.

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: 
Employee table:
+----+-------+--------+--------------+
| id | name  | salary | departmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 85000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
| 5  | Janet | 69000  | 1            |
| 6  | Randy | 85000  | 1            |
| 7  | Will  | 70000  | 1            |
+----+-------+--------+--------------+
Department table:
+----+-------+
| id | name  |
+----+-------+
| 1  | IT    |
| 2  | Sales |
+----+-------+
Output: 
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| IT         | Joe      | 85000  |
| IT         | Randy    | 85000  |
| IT         | Will     | 70000  |
| Sales      | Henry    | 80000  |
| Sales      | Sam      | 60000  |
+------------+----------+--------+
Explanation: 
In the IT department:
- Max earns the highest unique salary
- Both Randy and Joe earn the second-highest unique salary
- Will earns the third-highest unique salary

In the Sales department:
- Henry earns the highest salary
- Sam earns the second-highest salary
- There is no third-highest salary as there are only two employees
````
### Problem 42 Solution and Explanation:

```sql
SELECT Department, Employee, Salary
FROM (
    SELECT 
        d.name AS Department,
        e.name AS Employee,
        e.salary AS Salary,
        DENSE_RANK() OVER (PARTITION BY d.name ORDER BY Salary DESC) AS rnk
    FROM Employee e
    JOIN Department d
    ON e.departmentId = d.id
) AS rnk_tbl
WHERE rnk <= 3;
```

**Solution:**

This SQL query retrieves information about the top 3 highest-paid employees in each department. The solution involves the use of the `DENSE_RANK()` window function to assign ranks to employees based on their salaries within each department.

- **`SELECT Department, Employee, Salary FROM (...):`**
  - Specifies the columns (`Department`, `Employee`, and `Salary`) that will be included in the final result set.

- **`SELECT d.name AS Department, e.name AS Employee, e.salary AS Salary, DENSE_RANK() OVER (PARTITION BY d.name ORDER BY Salary DESC) AS rnk FROM Employee e JOIN Department d ON e.departmentId = d.id):`**
  - Creates a subquery that retrieves information about each employee, including the department name (`Department`), employee name (`Employee`), salary (`Salary`), and the dense rank (`rnk`) based on the salary within each department.
  - The `PARTITION BY` clause ensures that the ranking is done separately for each department.
  - The `ORDER BY` clause orders employees within each department by salary in descending order.

- **`AS rnk_tbl WHERE rnk <= 3;`**
  - Filters the results to include only rows where the dense rank is less than or equal to 3, meaning only the top 3 highest-paid employees in each department will be included in the final result.

**Explanation:**

The query aims to identify the top 3 highest-paid employees in each department. It achieves this by using the `DENSE_RANK()` window function, which assigns ranks to employees based on their salaries within each department. The subquery retrieves detailed information about employees and their ranks, and the outer query filters the results to include only those with ranks up to 3.

- The `JOIN` clause connects the `Employee` and `Department` tables based on the `departmentId`.
- The `DENSE_RANK()` function provides a ranking for each employee within their respective departments based on their salaries.
- The outer query then selects and displays the `Department`, `Employee`, and `Salary` columns for employees with ranks up to 3 in each department.

In summary, this SQL query provides a solution to identify the top 3 highest-paid employees in each department. The result set includes columns `Department`, `Employee`, and `Salary`.

---

### Problem 43: Fix Names in a Table

````
Table: Users

+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| user_id        | int     |
| name           | varchar |
+----------------+---------+
user_id is the primary key (column with unique values) for this table.
This table contains the ID and the name of the user. The name consists of only lowercase and uppercase characters.
 

Write a solution to fix the names so that only the first character is uppercase and the rest are lowercase.

Return the result table ordered by user_id.

The result format is in the following example.

 

Example 1:

Input: 
Users table:
+---------+-------+
| user_id | name  |
+---------+-------+
| 1       | aLice |
| 2       | bOB   |
+---------+-------+
Output: 
+---------+-------+
| user_id | name  |
+---------+-------+
| 1       | Alice |
| 2       | Bob   |
+---------+-------+
````
### Problem 43 Solution and Explanation:

```sql
SELECT user_id, CONCAT(UPPER(LEFT(name, 1)), LCASE(SUBSTRING(name, 2))) AS name
FROM Users
ORDER BY user_id;
```

**Solution:**

This SQL query modifies the representation of the `name` column in the `Users` table. It combines the `user_id` and a modified version of the `name` column. The modification involves capitalizing the first letter of each name.

- **`SELECT user_id, CONCAT(UPPER(LEFT(name, 1)), LCASE(SUBSTRING(name, 2))) AS name FROM Users:`**
  - Selects the `user_id` column and constructs a modified `name` column.
  - `UPPER(LEFT(name, 1))` converts the first letter of each name to uppercase.
  - `LCASE(SUBSTRING(name, 2))` converts the remaining part of each name to lowercase.
  - `CONCAT` combines the uppercase first letter and the lowercase remaining part.

- **`ORDER BY user_id;`**
  - Orders the result set based on the `user_id` column in ascending order.

**Explanation:**

The query aims to create a modified version of the `name` column, where the first letter of each name is capitalized while the rest of the name remains in lowercase. This is achieved by using string manipulation functions within the `CONCAT` function.

- `UPPER(LEFT(name, 1))`: Converts the first letter of each name to uppercase.
- `LCASE(SUBSTRING(name, 2))`: Converts the remaining part of each name (starting from the second character) to lowercase.

The `CONCAT` function combines these two modified parts to create the desired representation of the `name`. The result set includes both the `user_id` and the modified `name` columns.

The `ORDER BY user_id` ensures that the final result set is ordered based on the `user_id` in ascending order.

In summary, this SQL query provides a solution to display the `user_id` and a modified version of the `name` column where the first letter is capitalized and the rest is in lowercase.

---

### Problem 44: Patients With a Condition

````  
Table: Patients

+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| patient_id   | int     |
| patient_name | varchar |
| conditions   | varchar |
+--------------+---------+
patient_id is the primary key (column with unique values) for this table.
'conditions' contains 0 or more code separated by spaces. 
This table contains information of the patients in the hospital.
 

Write a solution to find the patient_id, patient_name, and conditions of the patients who have Type I Diabetes. Type I Diabetes always starts with DIAB1 prefix.

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: 
Patients table:
+------------+--------------+--------------+
| patient_id | patient_name | conditions   |
+------------+--------------+--------------+
| 1          | Daniel       | YFEV COUGH   |
| 2          | Alice        |              |
| 3          | Bob          | DIAB100 MYOP |
| 4          | George       | ACNE DIAB100 |
| 5          | Alain        | DIAB201      |
+------------+--------------+--------------+
Output: 
+------------+--------------+--------------+
| patient_id | patient_name | conditions   |
+------------+--------------+--------------+
| 3          | Bob          | DIAB100 MYOP |
| 4          | George       | ACNE DIAB100 | 
+------------+--------------+--------------+
Explanation: Bob and George both have a condition that starts with DIAB1.
````
### Problem 44 Solution and Explanation:

```sql
SELECT patient_id, patient_name, conditions
FROM Patients
WHERE conditions LIKE '% DIAB1%' OR conditions LIKE 'DIAB1%';
```

**Solution:**

This SQL query retrieves information about patients from the `Patients` table based on certain conditions related to the `conditions` column. The conditions specified in the `WHERE` clause involve filtering rows where the `conditions` column contains the substring ' DIAB1' anywhere or starts with 'DIAB1'.

- **`SELECT patient_id, patient_name, conditions FROM Patients:`**
  - Specifies the columns (`patient_id`, `patient_name`, and `conditions`) that will be included in the result set.

- **`WHERE conditions LIKE '% DIAB1%' OR conditions LIKE 'DIAB1%'`**
  - Filters the result set to include only those rows where the `conditions` column contains the substring ' DIAB1' anywhere (`LIKE '% DIAB1%'`) or starts with 'DIAB1' (`LIKE 'DIAB1%'`).

**Explanation:**

The query aims to identify patients whose medical conditions match certain criteria. The conditions specified in the `WHERE` clause include:

- `% DIAB1%`: This condition checks for rows where the `conditions` column contains the substring ' DIAB1' anywhere.
- `DIAB1%`: This condition checks for rows where the `conditions` column starts with 'DIAB1'.

The `OR` operator allows for rows that satisfy either of these conditions to be included in the final result set.

In summary, this SQL query provides a solution to retrieve information about patients whose medical conditions match the specified criteria. The result set includes columns `patient_id`, `patient_name`, and `conditions`.

---

### Problem 45: Delete Duplicate Emails

````    
Table: Person

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| email       | varchar |
+-------------+---------+
id is the primary key (column with unique values) for this table.
Each row of this table contains an email. The emails will not contain uppercase letters.
 

Write a solution to delete all duplicate emails, keeping only one unique email with the smallest id.

For SQL users, please note that you are supposed to write a DELETE statement and not a SELECT one.

For Pandas users, please note that you are supposed to modify Person in place.

After running your script, the answer shown is the Person table. The driver will first compile and run your piece of code and then show the Person table. The final order of the Person table does not matter.

The result format is in the following example.

 

Example 1:

Input: 
Person table:
+----+------------------+
| id | email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
| 3  | john@example.com |
+----+------------------+
Output: 
+----+------------------+
| id | email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
+----+------------------+
Explanation: john@example.com is repeated two times. We keep the row with the smallest Id = 1.
````
### Problem 45 Solution and Explanation:

```sql
DELETE p1 FROM person p1, person p2 
WHERE p1.email = p2.email AND p1.id > p2.id;
```

**Solution:**

This SQL query is designed to delete duplicate records from the `person` table based on a specific condition. It deletes records where there are duplicates with the same email, keeping only the record with the lower `id` value.

- **`DELETE p1 FROM person p1, person p2:`**
  - Specifies the table alias `p1` for the target table (`person`). This indicates that rows will be deleted from this table.
  - Uses a self-join with the table alias `p2` to compare records within the same table.

- **`WHERE p1.email = p2.email AND p1.id > p2.id;`**
  - Specifies the condition for deletion. It deletes rows where there are duplicates with the same email (`p1.email = p2.email`) but retains the record with the lower `id` value (`p1.id > p2.id`).

**Explanation:**

The query aims to remove duplicate records from the `person` table based on the `email` column. It uses a self-join to compare rows within the same table (`p1` and `p2`). The condition `p1.id > p2.id` ensures that only the record with the lower `id` value is retained, and the others are deleted.

- `p1.email = p2.email`: This condition ensures that only rows with matching email addresses are considered for deletion.

- `p1.id > p2.id`: This condition ensures that only the record with the lower `id` value is retained, and any records with a higher `id` value are deleted.

In summary, this SQL query provides a solution to delete duplicate records from the `person` table, keeping only the record with the lowest `id` value for each unique email. The `DELETE` statement specifies the table alias (`p1`) from which rows will be deleted.

---

# Hello, fellow learners! 🎉 
I'm excited to share my solutions and insights with you. I hope you find them valuable on your own learning path. 

Feel free to reach out if you have any questions or want to discuss any of the solutions. Let's embark on this journey of knowledge together! 📚💡

---
