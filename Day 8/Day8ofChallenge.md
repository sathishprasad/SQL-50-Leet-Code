# Day 7 of a 10-Day Learning Challenge 🚀

### Problem 36: Employees Whose Manager Left the Company

```` 
Table: Employees

+-------------+----------+
| Column Name | Type     |
+-------------+----------+
| employee_id | int      |
| name        | varchar  |
| manager_id  | int      |
| salary      | int      |
+-------------+----------+
In SQL, employee_id is the primary key for this table.
This table contains information about the employees, their salary, and the ID of their manager. Some employees do not have a manager (manager_id is null). 
 

Find the IDs of the employees whose salary is strictly less than $30000 and whose manager left the company. When a manager leaves the company, their information is deleted from the Employees table, but the reports still have their manager_id set to the manager that left.

Return the result table ordered by employee_id.

The result format is in the following example.

 

Example 1:

Input:  
Employees table:
+-------------+-----------+------------+--------+
| employee_id | name      | manager_id | salary |
+-------------+-----------+------------+--------+
| 3           | Mila      | 9          | 60301  |
| 12          | Antonella | null       | 31000  |
| 13          | Emery     | null       | 67084  |
| 1           | Kalel     | 11         | 21241  |
| 9           | Mikaela   | null       | 50937  |
| 11          | Joziah    | 6          | 28485  |
+-------------+-----------+------------+--------+
Output: 
+-------------+
| employee_id |
+-------------+
| 11          |
+-------------+

Explanation: 
The employees with a salary less than $30000 are 1 (Kalel) and 11 (Joziah).
Kalel's manager is employee 11, who is still in the company (Joziah).
Joziah's manager is employee 6, who left the company because there is no row for employee 6 as it was deleted.
````
### Problem 36 Solution and Explanation:

```sql
SELECT employee_id
FROM Employees
WHERE salary < 30000 AND manager_ID NOT IN (SELECT employee_id FROM Employees)
ORDER BY employee_id;
```

**Solution:**

This SQL query is designed to retrieve `employee_id` values from the Employees table based on the following conditions:
- The salary of the employee is less than 30000.
- The manager_ID of the employee is not in the list of employee_ids (subquery) from the Employees table.

The solution includes the following key components:

- **`SELECT employee_id FROM Employees:`**
  - Specifies the column `employee_id` that will be included in the result set.

- **`WHERE salary < 30000 AND manager_ID NOT IN (SELECT employee_id FROM Employees):`**
  - Filters the result set to include only those employees whose salary is less than 30000 and whose manager_ID is not in the list of employee_ids obtained from the subquery.

- **`ORDER BY employee_id;`**
  - Orders the result set by the `employee_id` in ascending order.

**Explanation:**

The query aims to identify employees who meet the specified criteria: having a salary less than 30000 and not being a manager (manager_ID not in the list of employee_ids). The subquery `(SELECT employee_id FROM Employees)` is used to obtain the list of all employee_ids.

- The `WHERE` clause filters employees based on the specified salary and manager_ID conditions.
- The `ORDER BY` clause ensures that the result set is ordered by `employee_id` in ascending order.

In summary, this SQL query provides a solution to retrieve employee_ids from the Employees table that meet the specified salary and manager_ID conditions. The result set includes the `employee_id` column.

---

### Problem 37: Exchange Seats

````
Table: Seat

+-------------+---------+
| Column Name | Type    |
+-------------+---------+
| id          | int     |
| student     | varchar |
+-------------+---------+
id is the primary key (unique value) column for this table.
Each row of this table indicates the name and the ID of a student.
id is a continuous increment.
 

Write a solution to swap the seat id of every two consecutive students. If the number of students is odd, the id of the last student is not swapped.

Return the result table ordered by id in ascending order.

The result format is in the following example.

 

Example 1:

Input: 
Seat table:
+----+---------+
| id | student |
+----+---------+
| 1  | Abbot   |
| 2  | Doris   |
| 3  | Emerson |
| 4  | Green   |
| 5  | Jeames  |
+----+---------+
Output: 
+----+---------+
| id | student |
+----+---------+
| 1  | Doris   |
| 2  | Abbot   |
| 3  | Green   |
| 4  | Emerson |
| 5  | Jeames  |
+----+---------+
Explanation: 
Note that if the number of students is odd, there is no need to change the last one's seat.
````
### Problem 37 Solution and Explanation:

```sql
SELECT 
    CASE 
        WHEN id = (SELECT MAX(id) FROM seat) AND id % 2 = 1 THEN id 
        WHEN id % 2 = 1 THEN id + 1
        ELSE id - 1
    END AS id,
    student
FROM seat
ORDER BY id;
```

**Solution:**

This SQL query is designed to rearrange the seat IDs and display the corresponding students from the `seat` table based on the following conditions:
- If the seat ID is the maximum ID in the `seat` table and is odd, keep the ID unchanged.
- If the seat ID is odd, increment it by 1.
- If the seat ID is even, decrement it by 1.

The solution includes the following key components:

- **`CASE WHEN ... END AS id:`**
  - Uses a `CASE` statement to determine the new value of the `id` column based on the specified conditions.

- **`student:`**
  - Represents the column `student` that will be included in the result set.

- **`FROM seat:`**
  - Specifies the source table as `seat` from which data will be retrieved.

- **`ORDER BY id;`**
  - Orders the result set by the `id` column in ascending order.

**Explanation:**

The query aims to rearrange seat IDs based on the specified conditions using a `CASE` statement. It calculates a new `id` for each row, and the result set includes the rearranged `id` and the corresponding `student`.

- The first condition preserves the maximum ID if it is odd.
- The second condition increments odd IDs by 1.
- The third condition decrements even IDs by 1.

The `ORDER BY` clause ensures that the result set is ordered by the rearranged `id` in ascending order.

In summary, this SQL query provides a solution to rearrange seat IDs in the `seat` table based on specific conditions. The result set includes the columns `id` and `student`.

---

### Problem 38: Movie Rating

````    
Table: Movies

+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| movie_id      | int     |
| title         | varchar |
+---------------+---------+
movie_id is the primary key (column with unique values) for this table.
title is the name of the movie.
 

Table: Users

+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| user_id       | int     |
| name          | varchar |
+---------------+---------+
user_id is the primary key (column with unique values) for this table.
 

Table: MovieRating

+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| movie_id      | int     |
| user_id       | int     |
| rating        | int     |
| created_at    | date    |
+---------------+---------+
(movie_id, user_id) is the primary key (column with unique values) for this table.
This table contains the rating of a movie by a user in their review.
created_at is the user's review date. 
 

Write a solution to:

Find the name of the user who has rated the greatest number of movies. In case of a tie, return the lexicographically smaller user name.
Find the movie name with the highest average rating in February 2020. In case of a tie, return the lexicographically smaller movie name.
The result format is in the following example.

 

Example 1:

Input: 
Movies table:
+-------------+--------------+
| movie_id    |  title       |
+-------------+--------------+
| 1           | Avengers     |
| 2           | Frozen 2     |
| 3           | Joker        |
+-------------+--------------+
Users table:
+-------------+--------------+
| user_id     |  name        |
+-------------+--------------+
| 1           | Daniel       |
| 2           | Monica       |
| 3           | Maria        |
| 4           | James        |
+-------------+--------------+
MovieRating table:
+-------------+--------------+--------------+-------------+
| movie_id    | user_id      | rating       | created_at  |
+-------------+--------------+--------------+-------------+
| 1           | 1            | 3            | 2020-01-12  |
| 1           | 2            | 4            | 2020-02-11  |
| 1           | 3            | 2            | 2020-02-12  |
| 1           | 4            | 1            | 2020-01-01  |
| 2           | 1            | 5            | 2020-02-17  | 
| 2           | 2            | 2            | 2020-02-01  | 
| 2           | 3            | 2            | 2020-03-01  |
| 3           | 1            | 3            | 2020-02-22  | 
| 3           | 2            | 4            | 2020-02-25  | 
+-------------+--------------+--------------+-------------+
Output: 
+--------------+
| results      |
+--------------+
| Daniel       |
| Frozen 2     |
+--------------+
Explanation: 
Daniel and Monica have rated 3 movies ("Avengers", "Frozen 2" and "Joker") but Daniel is smaller lexicographically.
Frozen 2 and Joker have a rating average of 3.5 in February but Frozen 2 is smaller lexicographically.
````
### Problem 38 Solution and Explanation:

```sql
(SELECT name AS results
FROM MovieRating JOIN Users USING(user_id)
GROUP BY name
ORDER BY COUNT(*) DESC, name
LIMIT 1)

UNION ALL

(SELECT title AS results
FROM MovieRating JOIN Movies USING(movie_id)
WHERE EXTRACT(YEAR_MONTH FROM created_at) = 202002
GROUP BY title
ORDER BY AVG(rating) DESC, title
LIMIT 1);
```

**Solution:**

This SQL query is designed to provide information about the highest-rated movie or user based on specific criteria. The solution includes two parts separated by `UNION ALL`, each part representing a different query:

1. **Highest-Rated User:**
   - **`SELECT name AS results FROM MovieRating JOIN Users USING(user_id) ...`**
   - Joins the `MovieRating` and `Users` tables based on the `user_id` column.
   - Groups the results by the `name` (user name).
   - Orders the groups by the count of ratings in descending order (`COUNT(*) DESC`) and then by the name.
   - Limits the result to the first row using `LIMIT 1`.

2. **Highest-Rated Movie in February 2020:**
   - **`SELECT title AS results FROM MovieRating JOIN Movies USING(movie_id) WHERE ...`**
   - Joins the `MovieRating` and `Movies` tables based on the `movie_id` column.
   - Filters the results to include only ratings from February 2020 (`EXTRACT(YEAR_MONTH FROM created_at) = 202002`).
   - Groups the results by the `title` (movie title).
   - Orders the groups by the average rating in descending order (`AVG(rating) DESC`) and then by the title.
   - Limits the result to the first row using `LIMIT 1`.

**Explanation:**

- The first part of the query focuses on users, calculating the count of ratings for each user and selecting the user with the highest count.
- The second part focuses on movies, considering only ratings from February 2020, and selects the movie with the highest average rating.

The `UNION ALL` operator combines the results of the two queries, ensuring that the result set includes both the highest-rated user and the highest-rated movie.

In summary, this SQL query provides a solution to find the highest-rated user and movie based on specific criteria. The result set includes the columns `results`, which may represent either user names or movie titles.

---

### Problem 39: Restaurant Growth

````
Table: Customer

+---------------+---------+
| Column Name   | Type    |
+---------------+---------+
| customer_id   | int     |
| name          | varchar |
| visited_on    | date    |
| amount        | int     |
+---------------+---------+
In SQL,(customer_id, visited_on) is the primary key for this table.
This table contains data about customer transactions in a restaurant.
visited_on is the date on which the customer with ID (customer_id) has visited the restaurant.
amount is the total paid by a customer.
 

You are the restaurant owner and you want to analyze a possible expansion (there will be at least one customer every day).

Compute the moving average of how much the customer paid in a seven days window (i.e., current day + 6 days before). average_amount should be rounded to two decimal places.

Return the result table ordered by visited_on in ascending order.

The result format is in the following example.

 

Example 1:

Input: 
Customer table:
+-------------+--------------+--------------+-------------+
| customer_id | name         | visited_on   | amount      |
+-------------+--------------+--------------+-------------+
| 1           | Jhon         | 2019-01-01   | 100         |
| 2           | Daniel       | 2019-01-02   | 110         |
| 3           | Jade         | 2019-01-03   | 120         |
| 4           | Khaled       | 2019-01-04   | 130         |
| 5           | Winston      | 2019-01-05   | 110         | 
| 6           | Elvis        | 2019-01-06   | 140         | 
| 7           | Anna         | 2019-01-07   | 150         |
| 8           | Maria        | 2019-01-08   | 80          |
| 9           | Jaze         | 2019-01-09   | 110         | 
| 1           | Jhon         | 2019-01-10   | 130         | 
| 3           | Jade         | 2019-01-10   | 150         | 
+-------------+--------------+--------------+-------------+
Output: 
+--------------+--------------+----------------+
| visited_on   | amount       | average_amount |
+--------------+--------------+----------------+
| 2019-01-07   | 860          | 122.86         |
| 2019-01-08   | 840          | 120            |
| 2019-01-09   | 840          | 120            |
| 2019-01-10   | 1000         | 142.86         |
+--------------+--------------+----------------+
Explanation: 
1st moving average from 2019-01-01 to 2019-01-07 has an average_amount of (100 + 110 + 120 + 130 + 110 + 140 + 150)/7 = 122.86
2nd moving average from 2019-01-02 to 2019-01-08 has an average_amount of (110 + 120 + 130 + 110 + 140 + 150 + 80)/7 = 120
3rd moving average from 2019-01-03 to 2019-01-09 has an average_amount of (120 + 130 + 110 + 140 + 150 + 80 + 110)/7 = 120
4th moving average from 2019-01-04 to 2019-01-10 has an average_amount of (130 + 110 + 140 + 150 + 80 + 110 + 130 + 150)/7 = 142.86
````
### Problem 39 Solution and Explanation:

```sql
SELECT
    visited_on,
    (
        SELECT SUM(amount)
        FROM customer
        WHERE visited_on BETWEEN DATE_SUB(c.visited_on, INTERVAL 6 DAY) AND c.visited_on
    ) AS amount,
    ROUND(
        (
            SELECT SUM(amount) / 7
            FROM customer
            WHERE visited_on BETWEEN DATE_SUB(c.visited_on, INTERVAL 6 DAY) AND c.visited_on
        ),
        2
    ) AS average_amount
FROM customer c
WHERE visited_on >= (
        SELECT DATE_ADD(MIN(visited_on), INTERVAL 6 DAY)
        FROM customer
    )
GROUP BY visited_on;
```

**Solution:**

This SQL query is designed to calculate the total amount spent (`amount`) and the average amount spent (`average_amount`) by customers over a 7-day window for each visit date. The solution includes the following key components:

- **`SELECT visited_on, ...`**
  - Specifies the column `visited_on` that will be included in the result set.

- **`(
        SELECT SUM(amount)
        FROM customer
        WHERE visited_on BETWEEN DATE_SUB(c.visited_on, INTERVAL 6 DAY) AND c.visited_on
    ) AS amount`**
  - Calculates the total amount spent by customers in a 7-day window before the current visit date (`c.visited_on`).

- **`ROUND(
        (
            SELECT SUM(amount) / 7
            FROM customer
            WHERE visited_on BETWEEN DATE_SUB(c.visited_on, INTERVAL 6 DAY) AND c.visited_on
        ),
        2
    ) AS average_amount`**
  - Calculates the average amount spent by customers over the same 7-day window and rounds it to 2 decimal places.

- **`FROM customer c`**
  - Specifies the source table as `customer` with the alias `c` from which data will be retrieved.

- **`WHERE visited_on >= (
        SELECT DATE_ADD(MIN(visited_on), INTERVAL 6 DAY)
        FROM customer
    )`**
  - Filters the result set to include only those rows where the `visited_on` date is greater than or equal to the earliest date plus 6 days.

- **`GROUP BY visited_on;`**
  - Groups the result set by the `visited_on` date.

**Explanation:**

The query aims to provide information about the total amount spent and the average amount spent by customers over a 7-day window for each visit date. The subqueries calculate these amounts within the specified date range, and the main query groups the results by the `visited_on` date.

- The first subquery calculates the total amount spent (`amount`) by customers in the 7-day window before each visit date.
- The second subquery calculates the average amount spent (`average_amount`) by customers over the same 7-day window.

The `ROUND` function is used to round the average amount to 2 decimal places.

In summary, this SQL query provides a solution to analyze customer spending patterns over a 7-day window for each visit date. The result set includes columns `visited_on`, `amount`, and `average_amount`.

----

### Problem 40: Friend Requests II: Who Has the Most Friends

````
Table: RequestAccepted

+----------------+---------+
| Column Name    | Type    |
+----------------+---------+
| requester_id   | int     |
| accepter_id    | int     |
| accept_date    | date    |
+----------------+---------+
(requester_id, accepter_id) is the primary key (combination of columns with unique values) for this table.
This table contains the ID of the user who sent the request, the ID of the user who received the request, and the date when the request was accepted.
 

Write a solution to find the people who have the most friends and the most friends number.

The test cases are generated so that only one person has the most friends.

The result format is in the following example.

 

Example 1:

Input: 
RequestAccepted table:
+--------------+-------------+-------------+
| requester_id | accepter_id | accept_date |
+--------------+-------------+-------------+
| 1            | 2           | 2016/06/03  |
| 1            | 3           | 2016/06/08  |
| 2            | 3           | 2016/06/08  |
| 3            | 4           | 2016/06/09  |
+--------------+-------------+-------------+
Output: 
+----+-----+
| id | num |
+----+-----+
| 3  | 3   |
+----+-----+
Explanation: 
The person with id 3 is a friend of people 1, 2, and 4, so he has three friends in total, which is the most number than any others.
 

Follow up: In the real world, multiple people could have the same most number of friends. Could you find all these people in this case?
````
### Problem 40 Solution and Explanation:

```sql
WITH base AS (
    SELECT requester_id AS id FROM RequestAccepted
    UNION ALL
    SELECT accepter_id AS id FROM RequestAccepted
)
SELECT id, COUNT(*) AS num
FROM base
GROUP BY id
ORDER BY num DESC
LIMIT 1;
```

**Solution:**

This SQL query is designed to find the user ID (`id`) with the highest number of occurrences in the combination of `requester_id` and `accepter_id` columns from the `RequestAccepted` table. The solution includes the following key components:

- **`WITH base AS (...)`**
  - Defines a Common Table Expression (CTE) named `base` that combines the `requester_id` and `accepter_id` columns from the `RequestAccepted` table using the `UNION ALL` operator.

- **`SELECT id, COUNT(*) AS num FROM base ...`**
  - Selects the `id` column from the `base` CTE and calculates the count of occurrences for each `id`.
  - Groups the results by the `id`.
  - Orders the groups in descending order based on the count (`num`).
  - Limits the result to the first row using `LIMIT 1`.

**Explanation:**

The query aims to identify the user ID with the highest number of occurrences in the combined list of `requester_id` and `accepter_id` from the `RequestAccepted` table.

- The `WITH` clause defines a CTE named `base` that combines the user IDs from both columns using `UNION ALL`. This creates a list of user IDs involved in either role (requester or accepter).

- The main query then counts the occurrences of each user ID (`id`) using the `COUNT(*)` function and groups the results by `id`.

- The `ORDER BY num DESC` clause orders the results in descending order based on the count of occurrences.

- The `LIMIT 1` ensures that only the user ID with the highest count is selected.

In summary, this SQL query provides a solution to find the user ID with the highest number of occurrences in the combined list of `requester_id` and `accepter_id` from the `RequestAccepted` table. The result set includes columns `id` and `num`.

---

# Hello, fellow learners! 🎉 
I'm excited to share my solutions and insights with you. I hope you find them valuable on your own learning path. 

Feel free to reach out if you have any questions or want to discuss any of the solutions. Let's embark on this journey of knowledge together! 📚💡

---

