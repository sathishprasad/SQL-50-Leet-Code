# Day 3 of a 10-Day Learning Challenge 🚀

### Problem 11: Employee Bonus 

````
Table: Employee

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| empId       | int     |
| name        | varchar |
| supervisor  | int     |
| salary      | int     |
+-------------+---------+
empId is the column with unique values for this table.
Each row of this table indicates the name and the ID of an employee in addition to their salary and the id of their manager.
 

Table: Bonus

+-------------+------+
| Column Name | Type |
+-------------+------+
| empId       | int  |
| bonus       | int  |
+-------------+------+
empId is the column of unique values for this table.
empId is a foreign key (reference column) to empId from the Employee table.
Each row of this table contains the id of an employee and their respective bonus.
 

Write a solution to report the name and bonus amount of each employee with a bonus less than 1000.

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: 
Employee table:
+-------+--------+------------+--------+
| empId | name   | supervisor | salary |
+-------+--------+------------+--------+
| 3     | Brad   | null       | 4000   |
| 1     | John   | 3          | 1000   |
| 2     | Dan    | 3          | 2000   |
| 4     | Thomas | 3          | 4000   |
+-------+--------+------------+--------+
Bonus table:
+-------+-------+
| empId | bonus |
+-------+-------+
| 2     | 500   |
| 4     | 2000  |
+-------+-------+
Output: 
+------+-------+
| name | bonus |
+------+-------+
| Brad | null  |
| John | null  |
| Dan  | 500   |
+------+-------+
````
### Problem 11 Solution and Explanation:
```sql
SELECT e.name, b.bonus
FROM Employee AS e
LEFT JOIN bonus AS b
ON e.empId = b.empID
WHERE b.bonus < 1000 OR b.bonus IS NULL;
```

**Let's break down this SQL query:**

1. **SELECT e.name, b.bonus:**
   - This part of the query indicates the columns we want to retrieve in the result set: `e.name` from the `Employee` table and `b.bonus` from the `bonus` table.

2. **FROM Employee AS e:**
   - Specifies the main table from which we want to retrieve data, and in this case, it's the `Employee` table. It is given the alias `e` for brevity.

3. **LEFT JOIN bonus AS b ON e.empId = b.empID:**
   - This is a LEFT JOIN clause that combines data from the `Employee` table (aliased as `e`) with the `bonus` table (aliased as `b`). It matches rows where the employee IDs (`empId` and `empID`) are equal.

4. **WHERE b.bonus < 1000 OR b.bonus IS NULL:**
   - This part is a condition that filters the joined rows. It includes rows where the bonus (`b.bonus`) is less than 1000 or where there is no corresponding bonus (`b.bonus IS NULL`).

In summary, this SQL query retrieves the names (`e.name`) and bonuses (`b.bonus`) for employees from the `Employee` table. It includes all employees, even those without a corresponding bonus record, due to the use of a LEFT JOIN. The WHERE clause further filters the results to include only those with a bonus less than 1000 or no bonus at all.

----

### Problem 12: Students and Examinations
````
Table: Students

+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| student_id    | int     |
| student_name  | varchar |
+---------------+---------+
student_id is the primary key (column with unique values) for this table.
Each row of this table contains the ID and the name of one student in the school.
 

Table: Subjects

+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| subject_name | varchar |
+--------------+---------+
subject_name is the primary key (column with unique values) for this table.
Each row of this table contains the name of one subject in the school.
 

Table: Examinations

+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| student_id   | int     |
| subject_name | varchar |
+--------------+---------+
There is no primary key (column with unique values) for this table. It may contain duplicates.
Each student from the Students table takes every course from the Subjects table.
Each row of this table indicates that a student with ID student_id attended the exam of subject_name.
 

Write a solution to find the number of times each student attended each exam.

Return the result table ordered by student_id and subject_name.

The result format is in the following example.

 

Example 1:

Input: 
Students table:
+------------+--------------+
| student_id | student_name |
+------------+--------------+
| 1          | Alice        |
| 2          | Bob          |
| 13         | John         |
| 6          | Alex         |
+------------+--------------+
Subjects table:
+--------------+
| subject_name |
+--------------+
| Math         |
| Physics      |
| Programming  |
+--------------+
Examinations table:
+------------+--------------+
| student_id | subject_name |
+------------+--------------+
| 1          | Math         |
| 1          | Physics      |
| 1          | Programming  |
| 2          | Programming  |
| 1          | Physics      |
| 1          | Math         |
| 13         | Math         |
| 13         | Programming  |
| 13         | Physics      |
| 2          | Math         |
| 1          | Math         |
+------------+--------------+
Output: 
+------------+--------------+--------------+----------------+
| student_id | student_name | subject_name | attended_exams |
+------------+--------------+--------------+----------------+
| 1          | Alice        | Math         | 3              |
| 1          | Alice        | Physics      | 2              |
| 1          | Alice        | Programming  | 1              |
| 2          | Bob          | Math         | 1              |
| 2          | Bob          | Physics      | 0              |
| 2          | Bob          | Programming  | 1              |
| 6          | Alex         | Math         | 0              |
| 6          | Alex         | Physics      | 0              |
| 6          | Alex         | Programming  | 0              |
| 13         | John         | Math         | 1              |
| 13         | John         | Physics      | 1              |
| 13         | John         | Programming  | 1              |
+------------+--------------+--------------+----------------+
Explanation: 
The result table should contain all students and all subjects.
Alice attended the Math exam 3 times, the Physics exam 2 times, and the Programming exam 1 time.
Bob attended the Math exam 1 time, the Programming exam 1 time, and did not attend the Physics exam.
Alex did not attend any exams.
John attended the Math exam 1 time, the Physics exam 1 time, and the Programming exam 1 time.
````
### Problem 12 Solution and Examination:
```sql
SELECT   a.student_id,
         a.student_name, 
         b.subject_name, 
         COUNT(c.subject_name) AS attended_exams 
FROM Students as a 
CROSS JOIN Subjects as b 
LEFT JOIN Examinations as c 
ON  a.student_id = c.student_id AND
    b.subject_name = c.subject_name  
GROUP BY 1,2,3
ORDER BY 1, 3;
```

**Let's break down this SQL query:**

1. **SELECT a.student_id, a.student_name, b.subject_name, COUNT(c.subject_name) AS attended_exams:**
   - This part of the query indicates the columns we want to retrieve in the result set: `a.student_id`, `a.student_name`, `b.subject_name`, and the count of attended exams (`COUNT(c.subject_name)`).

2. **FROM Students as a CROSS JOIN Subjects as b:**
   - Specifies the main tables from which we want to retrieve data: `Students` table (aliased as `a`) and `Subjects` table (aliased as `b`). It performs a CROSS JOIN, which generates the Cartesian product of the two tables, combining each row from `Students` with each row from `Subjects`.

3. **LEFT JOIN Examinations as c ON  a.student_id = c.student_id AND b.subject_name = c.subject_name:**
   - This is a LEFT JOIN clause that combines data from the result of the CROSS JOIN (aliased as `a` and `b`) with the `Examinations` table (aliased as `c`). It matches rows based on both the student ID and subject name.

4. **GROUP BY 1,2,3:**
   - This clause groups the result set by the first, second, and third columns, which are `a.student_id`, `a.student_name`, and `b.subject_name`.

5. **ORDER BY 1, 3:**
   - Orders the result set first by the student ID (`a.student_id`) and then by the subject name (`b.subject_name`) in ascending order.

In summary, this SQL query retrieves information about attended exams for each combination of students and subjects. It uses a CROSS JOIN to generate all possible combinations and then a LEFT JOIN to check if exams were attended for each combination. The result is grouped by student ID, student name, and subject name, with the count of attended exams, and it's ordered by student ID and subject name.

---

### Problem 13: Managers with atleast 5 direct reports

````
Table: Employee

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| name        | varchar |
| department  | varchar |
| managerId   | int     |
+-------------+---------+
id is the primary key (column with unique values) for this table.
Each row of this table indicates the name of an employee, their department, and the id of their manager.
If managerId is null, then the employee does not have a manager.
No employee will be the manager of themself.
 

Write a solution to find managers with at least five direct reports.

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: 
Employee table:
+-----+-------+------------+-----------+
| id  | name  | department | managerId |
+-----+-------+------------+-----------+
| 101 | John  | A          | null      |
| 102 | Dan   | A          | 101       |
| 103 | James | A          | 101       |
| 104 | Amy   | A          | 101       |
| 105 | Anne  | A          | 101       |
| 106 | Ron   | B          | 101       |
+-----+-------+------------+-----------+
Output: 
+------+
| name |
+------+
| John |
+------+
````
### Problem 13 Solution and Explanation:

```sql
SELECT name
FROM (
    SELECT  e1.id,
            e1.name, 
            COUNT(e1.id) AS n
    FROM Employee AS e1
    LEFT JOIN Employee AS e2
    ON e1.id = e2.managerId
    GROUP BY 1,2
    HAVING n > 4
) AS t1;
```

**Let's break down this SQL query**

1. **SELECT name:**
   - This part of the query indicates the column we want to retrieve in the result set: `name`.

2. **FROM (SELECT e1.id, e1.name, COUNT(e1.id) AS n FROM Employee AS e1 LEFT JOIN Employee AS e2 ON e1.id = e2.managerId GROUP BY 1,2 HAVING n > 4) AS t1:**
   - This is a subquery (an inner SELECT statement) that performs the following operations:
      - Retrieves `id`, `name`, and the count of occurrences (`COUNT(e1.id) AS n`) for each employee from the `Employee` table (aliased as `e1`).
      - Uses a LEFT JOIN to link each employee (`e1`) with their manager (`e2`) based on the `id` and `managerId` columns.
      - Groups the result by the first and second columns (`GROUP BY 1,2`), which are `id` and `name`.
      - Filters the grouped results to include only those with a count greater than 4 (`HAVING n > 4`).
   - The outer query then selects the `name` column from this result set.

In summary, this SQL query retrieves the names of employees who have more than 4 direct reports. It uses a subquery to count the number of direct reports for each employee and filters the result to include only those with more than 4 direct reports. The outer query then selects the names of these employees.

---

### Problem 14: Confirmation rate
````
Table: Signups

+----------------+----------+
| Column Name    | Type     |
+----------------+----------+
| user_id        | int      |
| time_stamp     | datetime |
+----------------+----------+
user_id is the column of unique values for this table.
Each row contains information about the signup time for the user with ID user_id.
 

Table: Confirmations

+----------------+----------+
| Column Name    | Type     |
+----------------+----------+
| user_id        | int      |
| time_stamp     | datetime |
| action         | ENUM     |
+----------------+----------+
(user_id, time_stamp) is the primary key (combination of columns with unique values) for this table.
user_id is a foreign key (reference column) to the Signups table.
action is an ENUM (category) of the type ('confirmed', 'timeout')
Each row of this table indicates that the user with ID user_id requested a confirmation message at time_stamp and that confirmation message was either confirmed ('confirmed') or expired without confirming ('timeout').
 

The confirmation rate of a user is the number of 'confirmed' messages divided by the total number of requested confirmation messages. The confirmation rate of a user that did not request any confirmation messages is 0. Round the confirmation rate to two decimal places.

Write a solution to find the confirmation rate of each user.

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: 
Signups table:
+---------+---------------------+
| user_id | time_stamp          |
+---------+---------------------+
| 3       | 2020-03-21 10:16:13 |
| 7       | 2020-01-04 13:57:59 |
| 2       | 2020-07-29 23:09:44 |
| 6       | 2020-12-09 10:39:37 |
+---------+---------------------+
Confirmations table:
+---------+---------------------+-----------+
| user_id | time_stamp          | action    |
+---------+---------------------+-----------+
| 3       | 2021-01-06 03:30:46 | timeout   |
| 3       | 2021-07-14 14:00:00 | timeout   |
| 7       | 2021-06-12 11:57:29 | confirmed |
| 7       | 2021-06-13 12:58:28 | confirmed |
| 7       | 2021-06-14 13:59:27 | confirmed |
| 2       | 2021-01-22 00:00:00 | confirmed |
| 2       | 2021-02-28 23:59:59 | timeout   |
+---------+---------------------+-----------+
Output: 
+---------+-------------------+
| user_id | confirmation_rate |
+---------+-------------------+
| 6       | 0.00              |
| 3       | 0.00              |
| 7       | 1.00              |
| 2       | 0.50              |
+---------+-------------------+
Explanation: 
User 6 did not request any confirmation messages. The confirmation rate is 0.
User 3 made 2 requests and both timed out. The confirmation rate is 0.
User 7 made 3 requests and all were confirmed. The confirmation rate is 1.
User 2 made 2 requests where one was confirmed and the other timed out. The confirmation rate is 1 / 2 = 0.5.
````
### Problem 14 Solution and Explanation:

```sql
SELECT  s.user_id,
        ROUND(SUM(CASE WHEN c.action='confirmed' THEN 1 ELSE 0 END)/COUNT(*),2) as confirmation_rate
FROM Signups as s
LEFT JOIN Confirmations as c
ON s.user_id = c.user_id
GROUP BY 1
ORDER BY 2;
```

**Let's break down this SQL query:**

1. **SELECT s.user_id, ROUND(SUM(CASE WHEN c.action='confirmed' THEN 1 ELSE 0 END)/COUNT(*),2) as confirmation_rate:**
   - This part of the query indicates the columns we want to retrieve in the result set: `s.user_id` and the calculated confirmation rate.
   - The confirmation rate is calculated by summing the cases where the confirmation action is 'confirmed' and dividing it by the total count of actions. The result is rounded to two decimal places.

2. **FROM Signups as s LEFT JOIN Confirmations as c ON s.user_id = c.user_id:**
   - Specifies the main tables from which we want to retrieve data: `Signups` table (aliased as `s`) and `Confirmations` table (aliased as `c`).
   - Performs a LEFT JOIN to match rows from `Signups` with corresponding rows from `Confirmations` based on the `user_id`.

3. **GROUP BY 1:**
   - Groups the result set by the first column (`s.user_id`), indicating that calculations should be performed for each unique `user_id`.

4. **ORDER BY 2:**
   - Orders the result set by the second column (`confirmation_rate`) in ascending order.

In summary, this SQL query calculates the confirmation rate for each user who signed up. It uses a LEFT JOIN to match signups with confirmations based on user IDs, calculates the confirmation rate, and then groups and orders the results accordingly.

---

### Problem 15: Not Boring Movies

````
Table: Cinema

+----------------+----------+
| Column Name    | Type     |
+----------------+----------+
| id             | int      |
| movie          | varchar  |
| description    | varchar  |
| rating         | float    |
+----------------+----------+
id is the primary key (column with unique values) for this table.
Each row contains information about the name of a movie, its genre, and its rating.
rating is a 2 decimal places float in the range [0, 10]
 

Write a solution to report the movies with an odd-numbered ID and a description that is not "boring".

Return the result table ordered by rating in descending order.

The result format is in the following example.

 

Example 1:

Input: 
Cinema table:
+----+------------+-------------+--------+
| id | movie      | description | rating |
+----+------------+-------------+--------+
| 1  | War        | great 3D    | 8.9    |
| 2  | Science    | fiction     | 8.5    |
| 3  | irish      | boring      | 6.2    |
| 4  | Ice song   | Fantacy     | 8.6    |
| 5  | House card | Interesting | 9.1    |
+----+------------+-------------+--------+
Output: 
+----+------------+-------------+--------+
| id | movie      | description | rating |
+----+------------+-------------+--------+
| 5  | House card | Interesting | 9.1    |
| 1  | War        | great 3D    | 8.9    |
+----+------------+-------------+--------+
Explanation: 
We have three movies with odd-numbered IDs: 1, 3, and 5. The movie with ID = 3 is boring so we do not include it in the answer.
````
### Problem 15 Solution and Explanation:

```sql
SELECT *
FROM Cinema
WHERE id % 2 != 0 AND description != 'boring'
ORDER BY rating DESC;
```

**Let's break down this SQL query:**

1. **SELECT *:**
   - This part of the query indicates that we want to retrieve all columns (`*`) from the `Cinema` table in the result set.

2. **FROM Cinema:**
   - Specifies the main table from which we want to retrieve data, and in this case, it's the `Cinema` table.

3. **WHERE id % 2 != 0 AND description != 'boring':**
   - This is the filtering condition. It includes only those rows where the `id` is an odd number (`id % 2 != 0`) and the `description` is not equal to 'boring'.

4. **ORDER BY rating DESC:**
   - Orders the result set by the `rating` column in descending order (`DESC`), meaning higher ratings will appear first.

In summary, this SQL query retrieves all columns from the `Cinema` table for rows where the `id` is an odd number, the `description` is not 'boring', and the results are ordered by the `rating` column in descending order.

---

# Hello, fellow learners! 🎉 
I'm excited to share my solutions and insights with you. I hope you find them valuable on your own learning path. 

Feel free to reach out if you have any questions or want to discuss any of the solutions. Let's embark on this journey of knowledge together! 📚💡

---
