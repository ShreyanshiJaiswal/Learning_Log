# Advanced String Functions Questions

This file has all the problems from the Advanced String Functions section of SQL50.

### [1667. Fix Names in a Table](https://leetcode.com/problems/fix-names-in-a-table/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT 
    user_id,
    CONCAT(
        UPPER(SUBSTRING(name, 1, 1)), 
        LOWER(SUBSTRING(name, 2))
    ) AS name
FROM Users
ORDER BY user_id;
```
## Thought Process
- The problem is about fixing the formatting of names in the table.  
- Each name should start with an uppercase letter and the rest should be lowercase.  
- To achieve this, we can take the first character of the name using `SUBSTRING(name, 1, 1)` and convert it to uppercase with `UPPER()`.  
- For the remaining characters, use `SUBSTRING(name, 2)` and convert them to lowercase with `LOWER()`.  
- Combine both parts using `CONCAT()` to form the corrected name.  
- Finally, order the output by `user_id` for a consistent result.  

### [1527. Patients With a Condition](https://leetcode.com/problems/patients-with-a-condition/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT patient_id, patient_name, conditions 
FROM Patients
WHERE conditions LIKE 'DIAB1%' 
   OR conditions LIKE '% DIAB1%';
```
## Thought Process
- We need to find all patients whose conditions include `"DIAB1"`.  
- Sometimes `"DIAB1"` can appear at the very beginning of the string, so we check with `LIKE 'DIAB1%'`.  
- Other times, it can appear after a space (not at the start), so we also check with `LIKE '% DIAB1%'`.  
- Using `OR` ensures we capture both cases.  
- Finally, select the required columns: `patient_id`, `patient_name`, and `conditions`.  

### [196. Delete Duplicate Emails](https://leetcode.com/problems/delete-duplicate-emails/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
DELETE FROM Person
WHERE id NOT IN (
    SELECT id
    FROM (
        SELECT MIN(id) AS id
        FROM Person
        GROUP BY email
    ) AS t
);
```
## Thought Process
- The goal is to **remove duplicate emails** but keep only one record for each email.  
- Use `GROUP BY email` to group records with the same email.  
- Within each group, keep the record with the smallest `id` using `MIN(id)`.  
- Subquery returns only these valid `id`s to keep.  
- Delete all rows in `Person` where `id` is **not in** this list.  
- Wrapping the grouped subquery in another derived table (`AS t`) avoids MySQL’s restriction on modifying and selecting from the same table simultaneously.  

### [176. Second Highest Salary](https://leetcode.com/problems/second-highest-salary/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT (
    SELECT DISTINCT salary
    FROM Employee
    ORDER BY salary DESC
    LIMIT 1 OFFSET 1
) AS SecondHighestSalary;
```
## Thought Process
- The task is to return the **second highest salary** from the `Employee` table.  
- `DISTINCT salary` removes duplicates, ensuring unique salary values.  
- `ORDER BY salary DESC` arranges salaries from highest to lowest.  
- `LIMIT 1 OFFSET 1` skips the highest salary (offset 1) and fetches the next one (limit 1).  
- Wrapping the subquery in the outer `SELECT` allows the result to be returned as `SecondHighestSalary`.  
- If no second highest salary exists, the query naturally returns `NULL`.  

### [1484. Group Sold Products By The Date](https://leetcode.com/problems/group-sold-products-by-the-date/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT 
    sell_date,
    COUNT(DISTINCT product) AS num_sold,
    GROUP_CONCAT(DISTINCT product ORDER BY product SEPARATOR ',') AS products
FROM Activities
GROUP BY sell_date
ORDER BY sell_date;
```
## Thought Process
- The goal is to group products sold on the same date.  
- Use `GROUP BY sell_date` to aggregate data for each date.  
- `COUNT(DISTINCT product)` gives the number of unique products sold on that day.  
- `GROUP_CONCAT(DISTINCT product ORDER BY product SEPARATOR ',')` lists all unique products, ordered alphabetically, and joined into a single comma-separated string.  
- `ORDER BY sell_date` ensures the results are shown in chronological order.

### [1327. List the Products Ordered in a Period](https://leetcode.com/problems/list-the-products-ordered-in-a-period/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT 
    p.product_name,
    r.total_units AS unit
FROM 
    (SELECT 
         product_id, 
         SUM(unit) AS total_units
     FROM Orders
     WHERE order_date BETWEEN '2020-02-01' AND '2020-02-29'
     GROUP BY product_id
     HAVING SUM(unit) >= 100
    ) r
JOIN Products p 
ON r.product_id = p.product_id;
```
## Thought Process
- We need to find products with significant orders in February 2020.  
- Filter `Orders` table for dates between '2020-02-01' and '2020-02-29'.  
- Group by `product_id` and sum the `unit` to get total units per product.  
- Keep only products where total units >= 100 using `HAVING`.  
- Join with `Products` table to get the `product_name`.  
- Select product name and total units for the final output.  

