# Advanced Select and Joins Questions

This file has all the problems from the Basic Advanced Select and Joins section of SQL50.

### [1731. The Number of Employees Which Report to Each Employee](https://leetcode.com/problems/the-number-of-employees-which-report-to-each-employee/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT 
    m.employee_id,
    m.name,
    COUNT(e.employee_id) AS reports_count,
    ROUND(AVG(e.age)) AS average_age
FROM Employees e
JOIN Employees m
    ON e.reports_to = m.employee_id
GROUP BY m.employee_id, m.name
ORDER BY m.employee_id;
```
## Thought Process
- Goal: Find how many employees report to each manager and their average age.  
- Step 1: Use a **self-join** on the `Employees` table:  
  - `e` = employee (the one who reports)  
  - `m` = manager (the one being reported to).  
- Step 2: Match with `ON e.reports_to = m.employee_id`.  
- Step 3: For each manager, count how many employees report to them → `COUNT(e.employee_id)`.  
- Step 4: Calculate the average age of these reporting employees → `AVG(e.age)`, rounded with `ROUND()`.  
- Step 5: Group results by `m.employee_id` and `m.name` (since each manager’s summary is required).  
- Step 6: Order by `m.employee_id` for a clean ascending order list.  

📌 This ensures we see each manager, how many direct reports they have, and the average age of their team members.

### [1789. Primary Department for Each Employee](https://leetcode.com/problems/primary-department-for-each-employee/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT 
    employee_id,
    department_id
FROM Employee
WHERE primary_flag = 'Y'
   OR employee_id IN (
        SELECT employee_id
        FROM Employee
        GROUP BY employee_id
        HAVING COUNT(department_id) = 1
   );
```
## Thought Process
- The task is to return the primary department of each employee.  
- Two cases exist:
  1. If an employee has multiple departments, only the row with `primary_flag = 'Y'` should be selected.  
  2. If an employee has only one department, that department must be selected, even if its `primary_flag` is not `'Y'`.  
- To solve:
  - First, select rows where `primary_flag = 'Y'`.  
  - Next, add employees who belong to exactly one department using `GROUP BY employee_id HAVING COUNT(department_id) = 1`.  
- This ensures we capture both single-department employees and multi-department employees with a defined primary.  

### [610. Triangle Judgement](https://leetcode.com/problems/triangle-judgement/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT 
    x, y, z,
    CASE 
        WHEN x + y > z AND x + z > y AND y + z > x 
        THEN 'Yes'
        ELSE 'No'
    END AS triangle
FROM Triangle;
```
## Thought Process
- Goal: Check if three sides can form a valid triangle.  
- Rule: A triangle is valid only if the sum of any two sides is strictly greater than the third side.  
- Translate this into SQL with a `CASE` statement:
  - `WHEN x + y > z AND x + z > y AND y + z > x THEN 'Yes'`
  - Otherwise `'No'`.  
- Simply return the sides `x, y, z` along with the computed result as `triangle`.  

### [180. Consecutive Numbers](https://leetcode.com/problems/consecutive-numbers/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT DISTINCT t1.num AS ConsecutiveNums
FROM Logs t1
JOIN Logs t2 ON t1.id = t2.id - 1
JOIN Logs t3 ON t1.id = t3.id - 2
WHERE t1.num = t2.num 
  AND t2.num = t3.num;
```
## Thought Process
- Goal: Find numbers that appear consecutively **at least 3 times** in the `Logs` table.  
- Approach:
  - Compare each row (`t1`) with the next row (`t2`) and the row after that (`t3`), based on `id`.  
  - Condition: If `t1.num = t2.num = t3.num`, then that number appears 3 times in a row.  
  - Use `DISTINCT` to avoid duplicates in the result.  
- Output: Column `ConsecutiveNums` with all such numbers.  

### [1164. Product Price at a Given Date](https://leetcode.com/problems/product-price-at-a-given-date/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT p.product_id,
       COALESCE(latest.new_price, 10) AS price
FROM (SELECT DISTINCT product_id FROM Products) p
LEFT JOIN (
    SELECT product_id, new_price
    FROM Products
    WHERE (product_id, change_date) IN (
        SELECT product_id, MAX(change_date)
        FROM Products
        WHERE change_date <= '2019-08-16'
        GROUP BY product_id
    )
) latest
ON p.product_id = latest.product_id;
```
## Thought Process
1. **Initial Condition**
   - Every product starts with a default price = 10.

2. **Requirement**
   - If a product has at least one price change **on or before** `2019-08-16`, take the **latest change** (closest to that date).
   - If no change exists before that date → fallback to default price = 10.

3. **Step-by-step reasoning**
   - Extract all product_id’s (ensures we cover those with no price changes too).
   - For each product:
     - Find the `MAX(change_date)` ≤ `2019-08-16`.
     - Use that date to fetch the corresponding `new_price`.
   - If a product has no matching record, return `10` as default.
   - This requires:
     - Subquery with `MAX(change_date)` per product.
     - Join it back to Products to get the `new_price`.
     - Wrap with `COALESCE` to handle missing prices.

### [1204. Last Person to Fit in the Bus](https://leetcode.com/problems/last-person-to-fit-in-the-bus/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
WITH cte AS (
    SELECT person_name, 
           turn, 
           SUM(weight) OVER(ORDER BY turn ASC) AS running_weight
    FROM Queue
)
SELECT person_name
FROM cte
WHERE running_weight <= 1000
ORDER BY turn DESC
LIMIT 1;
```
## Thought Process

1. **What we need**
   - People are in a queue with `turn` and `weight`.
   - The bus can carry at most **1000** total weight.
   - We want the **last person** who can still board without the total going over 1000.

2. **How to think about it**
   - People board in order of `turn`, so keep a **running total** of weight as we move through the queue.
   - If the running total after someone boards is **≤ 1000**, they fit; otherwise they don’t.

3. **Plan**
   - Use a window function: `SUM(weight) OVER (ORDER BY turn)` to compute the running total (`running_weight`) for each person.
   - Keep only rows where `running_weight ≤ 1000` (these people fit).
   - From those, pick the one with the **largest `turn`** — that’s the **last person** who fits.

4. **Why this works**
   - The running sum mirrors real boarding order.
   - Filtering by `≤ 1000` keeps only valid boarders.
   - Taking the highest `turn` among valid rows gives the final person who still fits.

### [1907. Count Salary Categories](https://leetcode.com/problems/count-salary-categories/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT 'Low Salary' AS category, COUNT(*) AS accounts_count
FROM Accounts
WHERE income < 20000

UNION ALL

SELECT 'Average Salary' AS category, COUNT(*) AS accounts_count
FROM Accounts
WHERE income BETWEEN 20000 AND 50000

UNION ALL

SELECT 'High Salary' AS category, COUNT(*) AS accounts_count
FROM Accounts
WHERE income > 50000;
```
## Thought Process — 1907. Count Salary Categories

1. **Goal**  
   - Group accounts into three salary categories: Low, Average, High.  
   - Count how many accounts fall into each category.

2. **How to do it**  
   - Use `WHERE` to filter accounts for each category:  
     - Low: income < 20000  
     - Average: income BETWEEN 20000 AND 50000  
     - High: income > 50000  
   - Use `COUNT(*)` to get the number of accounts in each group.

3. **Combine results**  
   - Use `UNION ALL` to merge all three category counts into a single result set.  
   - Add a string label (`'Low Salary'`, etc.) so the output clearly shows the category name.

4. **Why this works**  
   - Each SELECT handles one category.  
   - `UNION ALL` preserves all rows.  
   - The output shows category + count in a simple, readable format.

