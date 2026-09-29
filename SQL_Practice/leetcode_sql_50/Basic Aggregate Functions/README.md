# Basic Aggregate Functions Questions

This file has all the problems from the Basic Aggregate Functions section of SQL50.

### [620. Not Boring Movies](https://leetcode.com/problems/not-boring-movies/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT id, movie, description, rating
FROM Cinema
WHERE id % 2 != 0
  AND description != 'boring'
ORDER BY rating DESC;
```
## Thought Process
- Keep only rows with **odd** `id` → `id % 2 != 0`.
- Exclude movies where `description` is `'boring'`.
- Return the needed columns: `id, movie, description, rating`.
- Sort by `rating` in **descending** order to show higher-rated movies first.

### [1251. Average Selling Price](https://leetcode.com/problems/average-selling-price/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT p.product_id, 
       ROUND(IFNULL(SUM(u.units * p.price) / SUM(u.units), 0), 2) AS average_price
FROM Prices p
LEFT JOIN UnitsSold u 
  ON p.product_id = u.product_id
 AND u.purchase_date BETWEEN p.start_date AND p.end_date
GROUP BY p.product_id;
```
## Thought Process
- We need **average selling price** = (total revenue) ÷ (total units sold).
- `total revenue` → `SUM(u.units * p.price)`.
- `total units` → `SUM(u.units)`.
- Use `LEFT JOIN` because some products may have **no sales**, still should appear.
- Match sales only when `purchase_date` falls between product’s `start_date` and `end_date`.
- Handle nulls with `IFNULL(..., 0)` so products without sales show `0`.
- Use `ROUND(..., 2)` to format to **2 decimal places**.
- Group results by `product_id`.

### [1075. Project Employees I](https://leetcode.com/problems/project-employees-i/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT p.project_id, 
       ROUND(AVG(e.experience_years), 2) AS average_years
FROM Project p
JOIN Employee e 
  ON p.employee_id = e.employee_id
GROUP BY p.project_id;
```
## Thought Process
- We want the **average experience years of employees per project**.
- The link table is `Project` (contains `project_id` and `employee_id`).
- Join with `Employee` table using `employee_id` to fetch experience.
- Apply `AVG(e.experience_years)` to get mean experience per project.
- Use `ROUND(..., 2)` for 2 decimal precision.
- Use `JOIN` instead of `LEFT JOIN` because every `Project.employee_id` is guaranteed to exist in `Employee`.
- Group by `project_id` to compute per project.

### [1633. Percentage of Users Attended a Contest](https://leetcode.com/problems/percentage-of-users-attended-a-contest/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT r.contest_id, 
       ROUND(COUNT(DISTINCT r.user_id) * 100 / (SELECT COUNT(user_id) FROM Users), 2) AS percentage
FROM Register r
LEFT JOIN Users u
       ON r.user_id = u.user_id
GROUP BY r.contest_id
ORDER BY percentage DESC, r.contest_id ASC;
```
## Thought Process
- We need the **percentage of users who registered for each contest**.
- `Register` table: contest registrations → `contest_id`, `user_id`.
- `Users` table: all users → used to get **total users**.
- Formula: `(registered_users / total_users) * 100`.
- Use `COUNT(DISTINCT r.user_id)` to avoid double-counting if a user registered multiple times for same contest.
- Use subquery `(SELECT COUNT(user_id) FROM Users)` to fetch denominator (total users).
- Apply `ROUND(..., 2)` for two decimal precision.
- Group by `contest_id` to compute percentage per contest.
- Order by percentage descending, then contest_id ascending as required.

### [1211. Queries Quality and Percentage](https://leetcode.com/problems/queries-quality-and-percentage/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT query_name, 
       ROUND(AVG(rating/position), 2) AS quality, 
       ROUND(SUM(rating < 3) * 100 / COUNT(rating), 2) AS poor_query_percentage
FROM Queries
GROUP BY query_name;
```
## Thought Process
- We need two metrics **per query_name**:
  1. `quality` → average of `rating / position`.
  2. `poor_query_percentage` → % of queries with `rating < 3`.
- For **quality**, use `AVG(rating/position)`.
- For **poor_query_percentage**, count how many rows satisfy `rating < 3`.  
  - In SQL, `(rating < 3)` evaluates to `1` (true) or `0` (false).  
  - So `SUM(rating < 3)` gives number of poor queries.  
  - Divide by total queries `COUNT(rating)`.  
  - Multiply by `100` for percentage.
- Use `ROUND(..., 2)` to get two decimal precision.
- `GROUP BY query_name` since calculation is per query.

### [1193. Monthly Transactions I](https://leetcode.com/problems/monthly-transactions-i/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT 
    DATE_FORMAT(trans_date, '%Y-%m') AS month,
    country,
    COUNT(id) AS trans_count,
    SUM(CASE WHEN state = 'approved' THEN 1 ELSE 0 END) AS approved_count,
    SUM(amount) AS trans_total_amount,
    SUM(CASE WHEN state = 'approved' THEN amount ELSE 0 END) AS approved_total_amount
FROM Transactions
GROUP BY DATE_FORMAT(trans_date, '%Y-%m'), country;
```
## Thought Process
- We need monthly statistics of transactions by **country**.
- Extract the **month** from `trans_date` using `DATE_FORMAT(trans_date, '%Y-%m')`.
- For each `(month, country)` pair:
  1. `trans_count` → total transactions (`COUNT(id)`).
  2. `approved_count` → approved transactions only → `SUM(CASE WHEN state='approved' THEN 1 ELSE 0 END)`.
  3. `trans_total_amount` → total transaction amount → `SUM(amount)`.
  4. `approved_total_amount` → approved transaction amounts only → `SUM(CASE WHEN state='approved' THEN amount ELSE 0 END)`.
- Use `GROUP BY DATE_FORMAT(trans_date, '%Y-%m'), country` since aggregation is per month and per country.

### [1174. Immediate Food Delivery II](https://leetcode.com/problems/immediate-food-delivery-ii/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
WITH FirstOrders AS (
    SELECT 
        customer_id,
        MIN(order_date) AS first_order_date
    FROM Delivery
    GROUP BY customer_id
)
SELECT 
    ROUND(
        100.0 * SUM(CASE 
                        WHEN d.order_date = d.customer_pref_delivery_date 
                        THEN 1 
                        ELSE 0 
                    END) / COUNT(*), 
        2
    ) AS immediate_percentage
FROM Delivery d
JOIN FirstOrders f
    ON d.customer_id = f.customer_id
   AND d.order_date = f.first_order_date;
```
### Thought Process
- First, the problem is asking about the **first order of each customer** and whether that first order was delivered immediately (meaning `order_date = customer_pref_delivery_date`).
- So step 1 is: find the **first order date** for each customer. We can do that with `MIN(order_date)` grouped by `customer_id`.
- Step 2: once we know the first order date, we need to check if that specific order was delivered immediately.
- For that, we join the first-order table with the main `Delivery` table on both `customer_id` and `order_date`.
- Step 3: after the join, we count how many first orders were delivered immediately and divide it by total first orders.
- Finally, we multiply by 100 to make it a percentage and round it to 2 decimal places.

So the main idea is:
1. Get each customer’s first order date.  
2. Join it back with the delivery table.  
3. Check if the delivery was immediate.  
4. Calculate percentage = (immediate first orders ÷ all first orders) × 100. 

## [550. Game Play Analysis IV](https://leetcode.com/problems/game-play-analysis-iv/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
WITH FirstLogins AS (
  SELECT
    player_id,
    MIN(event_date) AS first_login_date
  FROM Activity
  GROUP BY player_id
)
SELECT
  ROUND(
    SUM(CASE WHEN Activity.player_id IS NOT NULL THEN 1 ELSE 0 END) * 1.0
    / COUNT(*),
    2
  ) AS fraction
FROM FirstLogins
LEFT JOIN Activity
  ON Activity.player_id = FirstLogins.player_id
 AND Activity.event_date = DATE_ADD(FirstLogins.first_login_date, INTERVAL 1 DAY);
```
### Thought Process
- First, we need to know when each player logged in for the first time. So, we take the minimum `event_date` for every player. That will be their first login date.  
- Now, we want to check whether that same player also logged in **exactly the next day** after their first login.  
- To do that, we join the `Activity` table with the result of first logins, matching the same player, but with `event_date = first_login_date + 1`.  
- After joining, if a player has a record in the join, that means they came back the very next day.  
- Finally, we calculate the fraction = (number of players who logged in next day) ÷ (total players).  
- Round this result to 2 decimal places. 
