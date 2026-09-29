# Subqueries Questions

This file has all the problems from the Subqueries section of SQL50.

### [1978. Employees Whose Manager Left the Company](https://leetcode.com/problems/employees-whose-manager-left-the-company/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT employee_id
FROM Employees
WHERE salary < 30000
  AND manager_id NOT IN (
      SELECT employee_id 
      FROM Employees
  )
ORDER BY employee_id;
```
## Thought Process

- The question wants employees whose salary is less than 30000.  
- Along with that, their manager should have left the company.  
- If a manager left, their `employee_id` will not be present in the table anymore.  
- So I need to check employees where `manager_id` is not in the list of current `employee_id`s.  
- At the same time, I apply the salary filter `< 30000`.  
- This means only those employees are selected who earn less than 30000 and whose manager is missing from the table.  
- Finally, I order the result by `employee_id` to get the output in proper order.  

### [626. Exchange Seats](https://leetcode.com/problems/exchange-seats/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT id,
       CASE 
            WHEN id % 2 = 1 AND id + 1 <= (SELECT MAX(id) FROM Seat)
                THEN (SELECT student FROM Seat s WHERE s.id = Seat.id + 1)
            WHEN id % 2 = 0
                THEN (SELECT student FROM Seat s WHERE s.id = Seat.id - 1)
            ELSE student
       END AS student
FROM Seat
ORDER BY id;
```
## Thought Process
- Swap students in adjacent seats (odd ↔ next, even ↔ previous).  
- If `id` is odd and the next seat exists, take the student from `id + 1`.  
- If `id` is even, take the student from `id - 1`.  
- If last seat is odd with no pair, keep the same student.  
- Use `CASE` and small subqueries to pick the neighbor’s student when swapping.  
- Order by `id` so output follows seat order.

### [1341. Movie Rating](https://leetcode.com/problems/movie-rating/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
(
    SELECT u.name AS results
    FROM Users u
    JOIN (
        SELECT user_id, COUNT(*) AS rating_count 
        FROM MovieRating 
        GROUP BY user_id
    ) r  
    ON u.user_id = r.user_id 
    ORDER BY r.rating_count DESC, u.name ASC
    LIMIT 1
)
UNION ALL
(
    SELECT m.title AS results
    FROM Movies m 
    JOIN (
        SELECT movie_id, AVG(rating) AS avg_rating 
        FROM MovieRating
        WHERE created_at >= '2020-02-01'
          AND created_at < '2020-03-01' 
        GROUP BY movie_id
    ) r 
    ON m.movie_id = r.movie_id
    ORDER BY r.avg_rating DESC, m.title ASC
    LIMIT 1
);
```
## Thought Process
- The question asks for **two results**:  
  1. The user who gave the most ratings.  
  2. The movie with the highest average rating in Feb 2020.  

- First part:  
  - Count ratings per user using `GROUP BY user_id`.  
  - Order by rating count (desc) and username (asc) to break ties.  
  - Limit to top 1 user.  

- Second part:  
  - Filter ratings only in Feb 2020.  
  - Calculate average rating per movie.  
  - Order by average rating (desc), then movie title (asc).  
  - Limit to top 1 movie.  

- Combine both queries with `UNION ALL` to show the two results together in one column named `results`.  

### [1321. Restaurant Growth](https://leetcode.com/problems/restaurant-growth/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT visited_on, amount_7d AS amount, average_amount
FROM (
    SELECT 
        visited_on,
        SUM(amount) OVER (
            ORDER BY visited_on
            ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
        ) AS amount_7d,
        ROUND(
            AVG(amount) OVER (
                ORDER BY visited_on
                ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
            ), 2
        ) AS average_amount,
        ROW_NUMBER() OVER (ORDER BY visited_on) AS row_num
    FROM (
        SELECT visited_on, SUM(amount) AS amount
        FROM Customer
        GROUP BY visited_on
    ) daily
) t
WHERE row_num >= 7
ORDER BY visited_on;
```
## Thought Process
- We need to calculate the **7-day rolling sum and average** of customer spending.  

- Step 1:  
  - First, aggregate the raw `Customer` table by `visited_on` to get daily totals.  

- Step 2:  
  - Use window functions:  
    - `SUM(amount) OVER (ROWS BETWEEN 6 PRECEDING AND CURRENT ROW)` → gives the rolling 7-day total.  
    - `AVG(amount) OVER (ROWS BETWEEN 6 PRECEDING AND CURRENT ROW)` → gives the rolling 7-day average.  
  - Round the average to 2 decimals.  

- Step 3:  
  - Assign row numbers ordered by `visited_on`.  
  - Exclude the first 6 rows (`WHERE row_num >= 7`) since a full 7-day window is required.  

- Step 4:  
  - Output columns: `visited_on`, total amount in 7 days, and average.  
  - Order by `visited_on` for chronological reporting.  

### [602. Friend Requests II: Who Has the Most Friends](https://leetcode.com/problems/friend-requests-ii-who-has-the-most-friends/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT person AS id, COUNT(DISTINCT friend) AS num
FROM (
    SELECT requester_id AS person, accepter_id AS friend
    FROM RequestAccepted
    UNION ALL
    SELECT accepter_id AS person, requester_id AS friend
    FROM RequestAccepted
) t
GROUP BY person
ORDER BY num DESC
LIMIT 1;
```
## Thought Process
- We need to find the person with the **most friends**.  

- Step 1:  
  - Each row in `RequestAccepted` shows a friendship.  
  - Friendship is **bi-directional**, so we consider both `(requester, accepter)` and `(accepter, requester)` using `UNION ALL`.  

- Step 2:  
  - Count **distinct friends** for each person using `COUNT(DISTINCT friend)`.  

- Step 3:  
  - Group by `person` to aggregate their total friends.  

- Step 4:  
  - Order by `num DESC` to get the person with the most friends on top.  
  - Limit to 1 since we only need the top friend count.  

### [585. Investments in 2016](https://leetcode.com/problems/investments-in-2016/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT 
    ROUND(SUM(tiv_2016), 2) AS tiv_2016
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
## Thought Process
- We need the total `tiv_2016` for certain policies under specific conditions.  

- Step 1:  
  - Policy’s `tiv_2015` should appear more than once across the table.  
  - Use `GROUP BY tiv_2015 HAVING COUNT(*) > 1` to find those values.  

- Step 2:  
  - The location `(lat, lon)` must be **unique** (only one record at that location).  
  - Use `GROUP BY lat, lon HAVING COUNT(*) = 1` for that condition.  

- Step 3:  
  - Filter the main table with both conditions using `WHERE`.  

- Step 4:  
  - Sum the `tiv_2016` values of the qualifying records.  
  - Round to 2 decimal places as required.  

### [185. Department Top Three Salaries](https://leetcode.com/problems/department-top-three-salaries/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT d.name AS Department,
       e.name AS Employee,
       e.salary AS Salary
FROM (
    SELECT id,
           name,
           salary,
           departmentId,
           DENSE_RANK() OVER (PARTITION BY departmentId ORDER BY salary DESC) AS rnk
    FROM Employee
) e
JOIN Department d
  ON e.departmentId = d.id
WHERE e.rnk <= 3;
```
## Thought Process
- The task is to find the **top 3 salaries per department**.  
- We need the top 3 salaries for each department, so ranking salaries within each department is the key.  
- Using `DENSE_RANK()` makes sense because it avoids skipping ranks when salaries tie. For example, if two people share the same highest salary, they both get rank 1, and the next person gets rank 2.  
- Partitioning by `departmentId` ensures ranking happens separately for each department, not across all employees.  
- Ordering by `salary DESC` makes higher salaries get smaller rank numbers (1 is highest).  
- After ranking, we filter to only those rows where the rank is `<= 3`, so we only keep the top 3 salaries per department.  
- Since the Employee table has only department IDs, we need to join it with the Department table to fetch department names.  
- Finally, the query selects department name, employee name, and their salary for clear output.  
