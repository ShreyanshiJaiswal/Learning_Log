# Sorting and Grouping Questions

This file has all the problems from the Sorting and Grouping section of SQL50. 

### [2356. Number of Unique Subjects Taught by Each Teacher](https://leetcode.com/problems/number-of-unique-subjects-taught-by-each-teacher/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT 
    teacher_id, 
    COUNT(DISTINCT subject_id) AS cnt 
FROM Teacher 
GROUP BY teacher_id;
```
## Thought Process
- I need to find how many **different subjects** each teacher teaches.  
- The `DISTINCT subject_id` ensures no duplicate subjects are counted.  
- Use `COUNT(DISTINCT subject_id)` to calculate unique subjects.  
- Apply `GROUP BY teacher_id` so the result is shown per teacher.

### [1141. User Activity for the Past 30 Days I](https://leetcode.com/problems/user-activity-for-the-past-30-days-i/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT 
    activity_date AS day,
    COUNT(DISTINCT user_id) AS active_users
FROM Activity
WHERE activity_date BETWEEN DATE('2019-06-28') AND DATE('2019-07-27')
GROUP BY activity_date;
```
## Thought Process
- The task is to find the **number of active users per day** in the past 30 days.  
- Since multiple activities by the same user on the same day shouldn’t be double counted, use `COUNT(DISTINCT user_id)`.  
- We only consider dates between `'2019-06-28'` and `'2019-07-27'`.  
- `GROUP BY activity_date` ensures results are calculated **per day**.  
- Aliased `activity_date` as `day` for clarity in output.

### [1070. Product Sales Analysis III](https://leetcode.com/problems/product-sales-analysis-iii/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT 
    s.product_id, 
    sub.first_year, 
    s.quantity, 
    s.price 
FROM Sales s
JOIN (
    SELECT 
        product_id, 
        MIN(year) AS first_year 
    FROM Sales 
    GROUP BY product_id
) AS sub
ON s.product_id = sub.product_id 
AND s.year = sub.first_year;
```
## Thought Process
- The goal is to find **each product’s first year of sale** along with its sales details.  
- First, find the **minimum year per product** using `MIN(year)` grouped by `product_id`.  
- Use this subquery as `sub` to identify the first year for each product.  
- Join the subquery with the main `Sales` table on both `product_id` and `year = first_year` to fetch the correct rows.  
- Select required columns: `product_id`, `first_year`, `quantity`, and `price`.

### [596. Classes With at Least 5 Students](https://leetcode.com/problems/classes-with-at-least-5-students/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT class 
FROM Courses 
GROUP BY class
HAVING COUNT(DISTINCT student) >= 5;
```
## Thought Process
- We need to find classes that have **at least 5 distinct students**.  
- `GROUP BY class` ensures we evaluate students per class.  
- `COUNT(DISTINCT student)` is used because the same student may appear multiple times.  
- `HAVING` is used (not `WHERE`) because filtering happens **after grouping**.  
- Finally, select only those classes where the count is `>= 5`.

### [1729. Find Followers Count](https://leetcode.com/problems/find-followers-count/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT user_id, 
       COUNT(DISTINCT follower_id) AS followers_count 
FROM Followers
GROUP BY user_id 
ORDER BY user_id ASC;
```
## Thought Process
- The task is to find how many **distinct followers** each user has.  
- `GROUP BY user_id` ensures aggregation per user.  
- `COUNT(DISTINCT follower_id)` avoids double-counting if a follower follows the same user multiple times.  
- We rename the column as `followers_count` for clarity.  
- `ORDER BY user_id ASC` ensures results are sorted by user ID.

### [619. Biggest Single Number](https://leetcode.com/problems/biggest-single-number/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT MAX(num) AS num
FROM (
    SELECT num
    FROM MyNumbers
    GROUP BY num
    HAVING COUNT(num) = 1
) AS t;
```
## Thought Process
- We need the **largest number that appears only once**.  
- Step 1: `GROUP BY num HAVING COUNT(num) = 1` filters numbers that appear exactly once.  
- Step 2: Wrap this in a subquery, then use `MAX(num)` to find the biggest among them.  
- If no number occurs exactly once, the result will be `NULL` (as required by the problem).  

### [1045. Customers Who Bought All Products](https://leetcode.com/problems/customers-who-bought-all-products/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT customer_id
FROM Customer c
GROUP BY customer_id
HAVING COUNT(DISTINCT c.product_key) = (
    SELECT COUNT(*) FROM Product
);
```
## Thought Process
- Goal: Find customers who purchased **every product** listed in `Product`.  
- Step 1: For each `customer_id`, count distinct products purchased → `COUNT(DISTINCT product_key)`.  
- Step 2: Compare this count with the **total number of products** in the `Product` table → `SELECT COUNT(*) FROM Product`.  
- Step 3: Only return those customers where both counts match (meaning they bought all products).
  
📌 If a product is not bought by a customer, their distinct count will be less → they won’t be included.
