# Basic Commands

This file has all the problems from the Basic Commands section of SQL Practice Queries.

### [All Products](https://www.codechef.com/practice/course/sql-case-studies-topic-wise/SQLBP01/problems/SQLPBP11)

```sql
SELECT * 
FROM Products;
```
## Thought Process  
- Step 1: Use the `SELECT *` command to fetch every column from the table.  
- Step 2: Specify the table name `Products` using the `FROM` clause.  
📌 This query returns the complete dataset from the `Products` table, displaying every product and its associated information.

### [High Price of Products](https://www.codechef.com/practice/course/sql-case-studies-topic-wise/SQLBP01/problems/SQLPBP12)

```sql
SELECT product_name, category 
FROM Products
WHERE price > 100.00;
```
## Thought Process
- Step 1: Use the `SELECT` statement to retrieve the `product_name` and `category` columns from the `Products` table.  
- Step 2: Apply the `WHERE` clause to filter only those products whose `price` is greater than 100.00.  
📌 This query displays the names and categories of all products priced above 100.

### [Average Salary](https://www.codechef.com/practice/course/sql-case-studies-topic-wise/SQLBP01/problems/SQLPBP14)

```sql
SELECT AVG(salary) AS avg_salary 
FROM Works;
```
## Thought Process
- Step 1: Use the `AVG()` function to calculate the average of the `salary` column from the `Works` table.  
- Step 2: Assign the result an alias `avg_salary` for better readability.  
📌 This query returns the average salary of all employees in the `Works` table.

### [Locate People](https://www.codechef.com/practice/course/sql-case-studies-topic-wise/SQLBP01/problems/SQLPBP15)

```sql
SELECT department_name, location 
FROM departments
WHERE location LIKE 'S%';
```
## Thought Process
- Step 1: Use the `SELECT` statement to fetch `department_name` and `location` columns from the `departments` table.  
- Step 2: Apply the `WHERE` clause with the `LIKE 'S%'` condition to filter locations starting with the letter 'S'.  
📌 This query displays all departments whose locations begin with the letter 'S'.

### [Distinct Companies](https://www.codechef.com/practice/course/sql-case-studies-topic-wise/SQLBP01/problems/SQLPBP13)

```sql
SELECT DISTINCT(company_name) 
FROM Works;
```
## Thought Process
- Step 1: Use the `DISTINCT` keyword to eliminate duplicate values from the `company_name` column.  
- Step 2: Retrieve unique company names from the `Works` table.  
📌 This query returns a list of all distinct company names where employees work.

### [Fiction Collection Size](https://www.codechef.com/practice/course/sql-case-studies-topic-wise/SQLBP01/problems/SQLPBP02)

```sql
SELECT COUNT(*) AS fiction_count 
FROM Books
WHERE genre = 'Fiction';
```
## Thought Process
- Step 1: Use the `COUNT(*)` function to count the total number of records in the `Books` table.  
- Step 2: Apply the `WHERE` clause to include only those rows where the `genre` is `'Fiction'`.  
📌 This query returns the total number of books that belong to the Fiction genre.

### [List of Movies with Ratings](https://www.codechef.com/practice/course/sql-case-studies-topic-wise/SQLBP01/problems/SQLPBP01)

```sql
SELECT Movie_name 
FROM Cinema
WHERE Rating > 7 AND Rating < 9;
```
## Thought Process
- Step 1: Use the `SELECT` statement to retrieve the `Movie_name` column from the `Cinema` table.  
- Step 2: Apply the `WHERE` clause to filter movies with ratings greater than 7 and less than 9.  
📌 This query displays the names of movies whose ratings fall between 7 and 9.

### [Handling NULL Values](https://www.codechef.com/practice/course/sql-case-studies-topic-wise/SQLBP01/problems/SQLPBP07)

```sql
SELECT book_id, title, author, published_year 
FROM Library
WHERE rating IS NULL;
```
## Thought Process
- Step 1: Use the `SELECT` statement to retrieve `book_id`, `title`, `author`, and `published_year` columns from the `Library` table.  
- Step 2: Apply the `WHERE` clause with the condition `rating IS NULL` to filter books that have no rating.  
📌 This query returns the details of all books in the library that do not have a rating value.

### [Salary of Employees](https://www.codechef.com/practice/course/sql-case-studies-topic-wise/SQLBP01/problems/SQLPBP08)

```sql
SELECT employee_name, company, salary 
FROM Employees
WHERE category = 'Full-Time'
ORDER BY salary DESC;
```
## Thought Process
- Step 1: Use the `SELECT` statement to fetch `employee_name`, `company`, and `salary` columns from the `Employees` table.  
- Step 2: Apply the `WHERE` clause to filter only those employees whose `category` is `'Full-Time'`.  
- Step 3: Use the `ORDER BY` clause with `salary DESC` to sort the results in descending order of salary.  
📌 This query lists all full-time employees along with their company names and salaries, arranged from highest to lowest salary.

### [Department of Each Employee](https://www.codechef.com/practice/course/sql-case-studies-topic-wise/SQLBP01/problems/SQLPBP09?tab=statement)

```sql
SELECT department, COUNT(*) AS total_employees 
FROM Employees
GROUP BY department;
```
## Thought Process
- Step 1: Use the `SELECT` statement to retrieve the `department` column from the `Employees` table.  
- Step 2: Apply the `COUNT(*)` function to count the number of employees in each department.  
- Step 3: Use the `GROUP BY` clause on `department` to group records by department name.  
📌 This query shows the total number of employees working in each department.

### [Article Views](https://www.codechef.com/practice/course/sql-case-studies-topic-wise/SQLBP01/problems/SQLPBP06)

```sql
SELECT author_id, author_name, publication_name 
FROM Views
WHERE view_count = 0
ORDER BY author_id ASC;
```
## Thought Process
- Step 1: Use the `SELECT` statement to fetch `author_id`, `author_name`, and `publication_name` from the `Views` table.  
- Step 2: Apply the `WHERE` clause with the condition `view_count = 0` to filter authors whose articles have zero views.  
- Step 3: Use `ORDER BY author_id ASC` to sort the results by author ID in ascending order.  
📌 This query returns the list of authors and their publications that have not received any views yet.

### [Player Performance Insights](https://www.codechef.com/practice/course/sql-case-studies-topic-wise/SQLBP01/problems/SQLPBP04?tab=statement)

```sql
SELECT DISTINCT p.player_name, p.score 
FROM Players p
JOIN Matches m ON m.winner = p.player_name
ORDER BY p.score DESC
LIMIT 3;
```
## Thought Process
- Step 1: Use the `JOIN` clause to combine the `Players` table (`p`) with the `Matches` table (`m`) based on the condition `m.winner = p.player_name`.  
- Step 2: Use `DISTINCT` to avoid duplicate player entries.  
- Step 3: Retrieve the `player_name` and `score` columns of players who have won matches.  
- Step 4: Order the results by `score` in descending order and use `LIMIT 3` to get the top three players.  
📌 This query displays the top 3 highest-scoring players who have won at least one match.

### [Player Details](https://www.codechef.com/practice/course/sql-case-studies-topic-wise/SQLBP01/problems/SQLPBP05?tab=statement)

```sql
SELECT m.match_id, m.player_1, m.player_2, m.winner, m.match_date, p.score
FROM Matches m
JOIN Players p ON m.winner = p.player_name
ORDER BY m.match_date DESC
LIMIT 5;
```
## Thought Process
- Step 1: Use the `JOIN` clause to combine the `Matches` table (`m`) with the `Players` table (`p`) based on the condition `m.winner = p.player_name`.  
- Step 2: Select relevant columns from both tables — `match_id`, `player_1`, `player_2`, `winner`, `match_date`, and `score`.  
- Step 3: Use `ORDER BY m.match_date DESC` to display the most recent matches first.  
- Step 4: Apply `LIMIT 5` to fetch only the latest five match records.  
📌 This query returns details of the five most recent matches along with the winner’s score.
