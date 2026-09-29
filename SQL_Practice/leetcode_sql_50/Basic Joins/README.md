# Basic Joins Questions

This file has all the problems from the **Basic Joins** section of SQL50.  

---

### [1378. Replace Employee ID With The Unique Identifier](https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT EmployeeUNI.unique_id, Employees.name 
FROM Employees 
LEFT JOIN EmployeeUNI 
ON Employees.id = EmployeeUNI.id;
```
## Thought Process
- We have two tables: `Employees` and `EmployeeUNI`.  
- Each employee has a unique ID in `EmployeeUNI` we need to match.  
- Use a `LEFT JOIN` to combine `Employees` with `EmployeeUNI` on `id`.  
- Select `unique_id` from `EmployeeUNI` and `name` from `Employees`.  
- This gives every employee with their corresponding unique identifier.

### [1068. Product Sales Analysis I](https://leetcode.com/problems/product-sales-analysis-i/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT Product.product_name, Sales.year, Sales.price 
FROM Sales 
JOIN Product 
ON Sales.product_id = Product.product_id;
```
## Thought Process
- We have two tables: `Sales` and `Product`.  
- We want each product's name along with its sales year and price.  
- Use a `JOIN` to connect `Sales` with `Product` on `product_id`.  
- Select `product_name` from `Product` and `year`, `price` from `Sales`.  
- This gives all products with their sales details.

### [1581. Customer Who Visited but Did Not Make Any Transactions](https://leetcode.com/problems/customer-who-visited-but-did-not-make-any-transactions/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT Visits.customer_id, COUNT(Visits.visit_id) AS count_no_trans
FROM Visits
LEFT JOIN Transactions
    ON Visits.visit_id = Transactions.visit_id
WHERE Transactions.transaction_id IS NULL
GROUP BY Visits.customer_id;
```
## Thought Process
- We have `Visits` and `Transactions` tables.  
- We need customers who visited but did **not make any transactions**.  
- Use a `LEFT JOIN` from `Visits` to `Transactions` on `visit_id`.  
- If `transaction_id` is `NULL`, it means no transaction happened for that visit.  
- Count the visits for each customer using `COUNT()` and group by `customer_id`.  
- This gives each customer with the number of visits without transactions.

### [197. Rising Temperature](https://leetcode.com/problems/rising-temperature/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT w1.id 
FROM Weather w1 
JOIN Weather w2  
    ON DATEDIFF(w1.recordDate, w2.recordDate) = 1 
    AND w1.temperature > w2.temperature;
```
## Thought Process
- The table `Weather` has daily temperature records.  
- We want days when the temperature **rose compared to the previous day**.  
- Join the table to itself (`w1` and `w2`) on the condition that the dates differ by 1 day.  
- Check if `w1.temperature > w2.temperature` to find rising temperature days.  
- Select the `id` of those days.

### [1661. Average Time of Process per Machine](https://leetcode.com/problems/average-time-of-process-per-machine/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT start_log.machine_id, 
       ROUND(AVG(end_log.timestamp - start_log.timestamp), 3) AS processing_time
FROM Activity start_log
JOIN Activity end_log
    ON start_log.process_id = end_log.process_id
    AND start_log.machine_id = end_log.machine_id
    AND start_log.activity_type = 'start' 
    AND end_log.activity_type = 'end'
GROUP BY start_log.machine_id;
```
## Thought Process
- The `Activity` table logs events with `start` and `end` types for processes on machines.  
- We want the **average processing time per machine**.  
- Join the table to itself:
  - Match same `process_id` and `machine_id`.  
  - Pair `start` events with `end` events.  
- Calculate the time difference (`end - start`) and take the average using `AVG()`.  
- Round the result to 3 decimal places.  
- Group the result by `machine_id` to get average time per machine.

### [577. Employee Bonus](https://leetcode.com/problems/employee-bonus/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT Employee.name, Bonus.bonus 
FROM Employee 
LEFT JOIN Bonus 
    ON Employee.empId = Bonus.empId 
WHERE bonus < 1000 OR bonus IS NULL;
```
## Thought Process
- We have `Employee` and `Bonus` tables.  
- Goal: find employees with **bonus less than 1000** or **no bonus recorded**.  
- Use `LEFT JOIN` to keep all employees, even if they don’t have a bonus.  
- Apply condition `bonus < 1000 OR bonus IS NULL` to filter.  
- Select employee `name` and `bonus`.

### [1280. Students and Examinations](https://leetcode.com/problems/students-and-examinations/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT s.student_id, 
       s.student_name, 
       sub.subject_name, 
       COUNT(e.subject_name) AS attended_exams
FROM Students s 
CROSS JOIN Subjects sub 
LEFT JOIN Examinations e 
       ON s.student_id = e.student_id
      AND sub.subject_name = e.subject_name
GROUP BY s.student_id, s.student_name, sub.subject_name
ORDER BY s.student_id, sub.subject_name;
```
## Thought Process
- We need to list each **student–subject pair**, even if the student never appeared for the exam.  
- Use **CROSS JOIN** between `Students` and `Subjects` to generate all possible combinations.  
- Then **LEFT JOIN** with `Examinations` to check if that student appeared for that subject.  
- `COUNT(e.subject_name)` counts how many times the student attended the exam (0 if never attended).  
- Group by `student_id`, `student_name`, and `subject_name` to get results per pair.  
- Order by `student_id` and `subject_name` for clean output.

### [570. Managers with at Least 5 Direct Reports](https://leetcode.com/problems/managers-with-at-least-5-direct-reports/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT e2.name
FROM Employee e1
JOIN Employee e2
     ON e1.managerId = e2.id
GROUP BY e2.id, e2.name
HAVING COUNT(e1.id) >= 5;
```
## Thought Process
- Find managers with at least 5 direct reports.  
- Use **self-join** on `Employee` table:  
  - `e1` → employees  
  - `e2` → managers  
- Condition: `e1.managerId = e2.id` to link employees to their managers.  
- Group by `e2.id`, `e2.name` to count reports per manager.  
- Filter with `HAVING COUNT(e1.id) >= 5`.  
- Select `e2.name` to get manager names.  

### [1934. Confirmation Rate](https://leetcode.com/problems/confirmation-rate/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT 
    s.user_id,
    ROUND(
        IFNULL(SUM(c.action = 'confirmed') / COUNT(c.action), 0),
        2
    ) AS confirmation_rate
FROM Signups s
LEFT JOIN Confirmations c
    ON s.user_id = c.user_id
GROUP BY s.user_id;
```
## Thought Process
- We have `Signups` and `Confirmations` tables.  
- Goal: calculate **confirmation rate** per user.  
- Use `LEFT JOIN` to include all users, even if they have no confirmations.  
- Count `confirmed` actions with `SUM(c.action = 'confirmed')`.  
- Divide by total actions `COUNT(c.action)` to get the rate.  
- Use `IFNULL(..., 0)` to handle users with no confirmations.  
- Round the result to 2 decimal places.  
- Group by `s.user_id` to get the rate per user.
