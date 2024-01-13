# Day 5 of a 10-Day Learning Challenge 🚀

### Problem 21: Game Play Analysis IV

````
Table: Activity

+--------------+---------+
| Column Name  | Type    |
+--------------+---------+
| player_id    | int     |
| device_id    | int     |
| event_date   | date    |
| games_played | int     |
+--------------+---------+
(player_id, event_date) is the primary key (combination of columns with unique values) of this table.
This table shows the activity of players of some games.
Each row is a record of a player who logged in and played a number of games (possibly 0) before logging out on someday using some device.
 

Write a solution to report the fraction of players that logged in again on the day after the day they first logged in, rounded to 2 decimal places. In other words, you need to count the number of players that logged in for at least two consecutive days starting from their first login date, then divide that number by the total number of players.

The result format is in the following example.

 

Example 1:

Input: 
Activity table:
+-----------+-----------+------------+--------------+
| player_id | device_id | event_date | games_played |
+-----------+-----------+------------+--------------+
| 1         | 2         | 2016-03-01 | 5            |
| 1         | 2         | 2016-03-02 | 6            |
| 2         | 3         | 2017-06-25 | 1            |
| 3         | 1         | 2016-03-02 | 0            |
| 3         | 4         | 2018-07-03 | 5            |
+-----------+-----------+------------+--------------+
Output: 
+-----------+
| fraction  |
+-----------+
| 0.33      |
+-----------+
Explanation: 
Only the player with id 1 logged back in after the first day he had logged in so the answer is 1/3 = 0.33
````
### Problem 21 Solution and Explanation:

```sql
SELECT
  ROUND(COUNT(DISTINCT player_id) / (SELECT COUNT(DISTINCT player_id) FROM Activity), 2) AS fraction
FROM Activity
WHERE
  (player_id, DATE_SUB(event_date, INTERVAL 1 DAY))
  IN (
    SELECT player_id, MIN(event_date) AS first_login FROM Activity GROUP BY player_id
  );
```

**Let's break down this SQL query:**

- **`SELECT ROUND(COUNT(DISTINCT player_id) / (SELECT COUNT(DISTINCT player_id) FROM Activity), 2) AS fraction:`**
  - This part of the query calculates the fraction of distinct player IDs relative to the total number of distinct player IDs in the Activity table. It rounds the result to two decimal places.

- **`FROM Activity:`**
  - Specifies the main table from which we want to retrieve data, and in this case, it's the Activity table.

- **`WHERE (player_id, DATE_SUB(event_date, INTERVAL 1 DAY)) IN (...):`**
  - This condition filters the rows based on a subquery that selects the earliest (`MIN`) `event_date` for each `player_id`. It uses a composite condition (`(player_id, DATE_SUB(event_date, INTERVAL 1 DAY))`) to ensure that both `player_id` and the date one day before the event match the earliest login for each player.

**In summary,** this SQL query calculates the fraction of distinct player IDs whose earliest login occurred one day before an event. It does so by filtering the rows to include only the earliest login for each player and then calculates the fraction of distinct player IDs relative to the total number of distinct player IDs in the Activity table, rounding the result to two decimal places.

----

### Problem 22: Number of Unique Subjects Taught by Each Teacher
````
Table: Teacher

+-------------+------+
| Column Name | Type |
+-------------+------+
| teacher_id  | int  |
| subject_id  | int  |
| dept_id     | int  |
+-------------+------+
(subject_id, dept_id) is the primary key (combinations of columns with unique values) of this table.
Each row in this table indicates that the teacher with teacher_id teaches the subject subject_id in the department dept_id.
 

Write a solution to calculate the number of unique subjects each teacher teaches in the university.

Return the result table in any order.

The result format is shown in the following example.

 

Example 1:

Input: 
Teacher table:
+------------+------------+---------+
| teacher_id | subject_id | dept_id |
+------------+------------+---------+
| 1          | 2          | 3       |
| 1          | 2          | 4       |
| 1          | 3          | 3       |
| 2          | 1          | 1       |
| 2          | 2          | 1       |
| 2          | 3          | 1       |
| 2          | 4          | 1       |
+------------+------------+---------+
Output:  
+------------+-----+
| teacher_id | cnt |
+------------+-----+
| 1          | 2   |
| 2          | 4   |
+------------+-----+
Explanation: 
Teacher 1:
  - They teach subject 2 in departments 3 and 4.
  - They teach subject 3 in department 3.
Teacher 2:
  - They teach subject 1 in department 1.
  - They teach subject 2 in department 1.
  - They teach subject 3 in department 1.
  - They teach subject 4 in department 1.
````
### Problem 22 Solution and Explanation:

```sql
SELECT teacher_id, 
       COUNT(DISTINCT(subject_id)) AS cnt
FROM Teacher
GROUP BY 1;
```

This SQL query retrieves the count of distinct subject IDs for each teacher from the Teacher table. Let's break down the components of the query:

- **`SELECT teacher_id, COUNT(DISTINCT(subject_id)) AS cnt:`**
  - This part of the query selects the `teacher_id` and calculates the count of distinct `subject_id` for each teacher. The result is aliased as `cnt`.

- **`FROM Teacher:`**
  - Specifies the main table from which we want to retrieve data, and in this case, it's the Teacher table.

- **`GROUP BY 1:`**
  - Groups the results by the first selected column (`teacher_id`). The `COUNT` function then operates on each unique teacher, providing the count of distinct subject IDs for each teacher.

**In summary,** this SQL query provides the count of distinct subject IDs for each teacher in the Teacher table. The result includes the `teacher_id` and the corresponding count (`cnt`) of distinct subjects.

---

### Problem 23: User Activity for the Past 30 Days I
````
Table: Activity

+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| user_id       | int     |
| session_id    | int     |
| activity_date | date    |
| activity_type | enum    |
+---------------+---------+
This table may have duplicate rows.
The activity_type column is an ENUM (category) of type ('open_session', 'end_session', 'scroll_down', 'send_message').
The table shows the user activities for a social media website. 
Note that each session belongs to exactly one user.
 

Write a solution to find the daily active user count for a period of 30 days ending 2019-07-27 inclusively. A user was active on someday if they made at least one activity on that day.

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: 
Activity table:
+---------+------------+---------------+---------------+
| user_id | session_id | activity_date | activity_type |
+---------+------------+---------------+---------------+
| 1       | 1          | 2019-07-20    | open_session  |
| 1       | 1          | 2019-07-20    | scroll_down   |
| 1       | 1          | 2019-07-20    | end_session   |
| 2       | 4          | 2019-07-20    | open_session  |
| 2       | 4          | 2019-07-21    | send_message  |
| 2       | 4          | 2019-07-21    | end_session   |
| 3       | 2          | 2019-07-21    | open_session  |
| 3       | 2          | 2019-07-21    | send_message  |
| 3       | 2          | 2019-07-21    | end_session   |
| 4       | 3          | 2019-06-25    | open_session  |
| 4       | 3          | 2019-06-25    | end_session   |
+---------+------------+---------------+---------------+
Output: 
+------------+--------------+ 
| day        | active_users |
+------------+--------------+ 
| 2019-07-20 | 2            |
| 2019-07-21 | 2            |
+------------+--------------+ 
Explanation: Note that we do not care about days with zero active users.
````
### Problem 23 Solution and Explanation:

```sql
SELECT  activity_date AS day, 
        COUNT(DISTINCT user_id) AS active_users
FROM Activity
WHERE (activity_date > "2019-06-27" AND activity_date <= "2019-07-27")
GROUP BY activity_date;
```

**Solution:**

This SQL query is designed to retrieve the count of distinct active users for each day within a specified date range from the Activity table. The solution is achieved through the following key components:

- **`SELECT activity_date AS day, COUNT(DISTINCT user_id) AS active_users:`**
  - Selects the `activity_date` column and calculates the count of distinct `user_id` for each day. The result is aliased as `day` for the activity date and `active_users` for the count of active users.

- **`FROM Activity:`**
  - Specifies the main table from which data is retrieved, and in this case, it's the Activity table.

- **`WHERE (activity_date > "2019-06-27" AND activity_date <= "2019-07-27"):`**
  - Filters the rows to include only those where the `activity_date` falls within the specified date range.

- **`GROUP BY activity_date;`**
  - Groups the results by the `activity_date` column. The `COUNT` function then operates on each unique activity date, providing the count of distinct active users for each day.

**Explanation:**

This query addresses the need to analyze the activity data within a specific time frame, identifying the count of distinct active users for each day. The `COUNT(DISTINCT user_id)` ensures that each user is counted only once per day, and the `GROUP BY activity_date` organizes the results by day.

In summary, this SQL query offers a solution to extract valuable insights into daily user activity within the specified date range from the Activity table. The result set includes the activity date (`day`) and the corresponding count (`active_users`) of distinct users for each day.

---

### Problem 24: Product Sales Analysis III

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
 

Write a solution to select the product id, year, quantity, and price for the first year of every product sold.

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
+------------+------------+----------+-------+
| product_id | first_year | quantity | price |
+------------+------------+----------+-------+ 
| 100        | 2008       | 10       | 5000  |
| 200        | 2011       | 15       | 9000  |
+------------+------------+----------+-------+
````
### Problem 24 Solution and Explanation:

```sql
SELECT product_id, year AS first_year, quantity, price
FROM sales
WHERE (product_id, year) IN (
    SELECT product_id, MIN(year)
    FROM sales
    GROUP BY product_id
);
```

**Solution:**

This SQL query is designed to retrieve specific information from the sales table, focusing on the first year of sales for each product. The solution is achieved through the following key components:

- **`SELECT product_id, year AS first_year, quantity, price:`**
  - Selects the `product_id`, `year` (aliased as `first_year`), `quantity`, and `price` columns.

- **`FROM sales:`**
  - Specifies the main table from which data is retrieved, and in this case, it's the sales table.

- **`WHERE (product_id, year) IN (...):`**
  - Filters the rows to include only those where the combination of `product_id` and `year` matches the minimum (`MIN`) year for each product.

- **`SELECT product_id, MIN(year) FROM sales GROUP BY product_id:`**
  - This subquery selects the minimum year for each product by grouping the sales table based on the `product_id`.

**Explanation:**

The query aims to identify and retrieve information about the first year of sales for each product. The subquery `(product_id, MIN(year))` is used to determine the minimum year for each product, and the outer query then filters rows based on this information.

In summary, this SQL query provides a solution to extract details about the first year of sales for each product from the sales table. The result set includes columns such as `product_id`, `first_year`, `quantity`, and `price` for the identified records.

---

### Problem 25: Classes More Than 5 Students

````
Table: Courses

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| student     | varchar |
| class       | varchar |
+-------------+---------+
(student, class) is the primary key (combination of columns with unique values) for this table.
Each row of this table indicates the name of a student and the class in which they are enrolled.
 

Write a solution to find all the classes that have at least five students.

Return the result table in any order.

The result format is in the following example.

 

Example 1:

Input: 
Courses table:
+---------+----------+
| student | class    |
+---------+----------+
| A       | Math     |
| B       | English  |
| C       | Math     |
| D       | Biology  |
| E       | Math     |
| F       | Computer |
| G       | Math     |
| H       | Math     |
| I       | Math     |
+---------+----------+
Output: 
+---------+
| class   |
+---------+
| Math    |
+---------+
Explanation: 
- Math has 6 students, so we include it.
- English has 1 student, so we do not include it.
- Biology has 1 student, so we do not include it.
- Computer has 1 student, so we do not include it.
````
### Problem 25 Solution and Explanation:

```sql
SELECT class
FROM Courses
GROUP BY 1
HAVING COUNT(class) > 4;
```

**Solution:**

This SQL query is designed to retrieve information about classes from the Courses table where the count of occurrences of each class is greater than 4. The solution is achieved through the following key components:

- **`SELECT class:`**
  - This part of the query specifies the column `class` that will be included in the result set.

- **`FROM Courses:`**
  - Specifies the main table from which data is retrieved, and in this case, it's the Courses table.

- **`GROUP BY 1:`**
  - Groups the results by the first selected column (`class`).

- **`HAVING COUNT(class) > 4;`**
  - Filters the grouped results to include only those where the count of occurrences of each class is greater than 4.

**Explanation:**

The query is used to identify and retrieve information about classes that have more than 4 occurrences in the Courses table. The `GROUP BY` clause groups the rows based on the `class` column, and the `HAVING` clause filters the results to include only those classes with a count greater than 4.

In summary, this SQL query provides a solution to find classes with a significant number of occurrences in the Courses table. The result set includes the `class` values for classes that meet the specified condition.

---

# Hello, fellow learners! 🎉 
I'm excited to share my solutions and insights with you. I hope you find them valuable on your own learning path. 

Feel free to reach out if you have any questions or want to discuss any of the solutions. Let's embark on this journey of knowledge together! 📚💡

---
