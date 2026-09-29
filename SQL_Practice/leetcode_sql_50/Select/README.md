# Select Questions

This file has all the problems from the **Select** section of SQL50.  
Each question has a link, my SQL solution, and a short thought process.

---

### [1757. Recyclable and Low Fat Products](https://leetcode.com/problems/recyclable-and-low-fat-products/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT product_id 
FROM Products 
WHERE low_fats = 'Y' AND recyclable = 'Y';
```
## Thought Process
- The table contains product details.
- We only want products that are **both** low fat and recyclable.
- That means two conditions should be true at the same time.
- I used the `WHERE` clause with:
    `low_fats = 'Y'`
    `recyclable = 'Y'`
- The `AND` ensures both conditions are satisfied together.
- Final output → product IDs that meet both conditions.

### [584. Find Customer Referee](https://leetcode.com/problems/find-customer-referee/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT name 
FROM Customer 
WHERE referee_id != 2 OR referee_id IS NULL;
```
## Thought Process
- The table has customer details including who referred them (`referee_id`).
- We want all customers **except the ones referred by customer 2**, and also include customers who were **not referred by anyone**.
- Used `WHERE referee_id != 2` to exclude referrals from customer 2.
- Used `OR referee_id IS NULL` to include customers without any referee.
- Final output → names of all customers meeting these conditions.

### [595. Big Countries](https://leetcode.com/problems/big-countries/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT name, population, area 
FROM World 
WHERE area >= 3000000 OR population >= 25000000;
```
## Thought Process
- The problem wants countries considered "big".  
- A country is big if **area ≥ 3,000,000** OR **population ≥ 25,000,000**.  
- We only need to return `name`, `population`, and `area`.  
- So we apply the condition with `OR` in the `WHERE` clause.
  
### [1148. Article Views I](https://leetcode.com/problems/article-views-i/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT DISTINCT(author_id) AS id 
FROM Views 
WHERE author_id = viewer_id 
ORDER BY id ASC;
```
## Thought Process
- The table `Views` contains `author_id` and `viewer_id`.  
- We need to find users who **viewed their own articles** → condition is `author_id = viewer_id`.  
- Since the same author may appear multiple times, we use `DISTINCT` to avoid duplicates.  
- Rename `author_id` as `id` (per problem requirement).  
- Finally, sort the result in ascending order of `id`.

### [1683. Invalid Tweets](https://leetcode.com/problems/invalid-tweets/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT tweet_id 
FROM Tweets 
WHERE LENGTH(content) > 15;
```
## Thought Process
- The table `Tweets` has `tweet_id` and `content`.  
- A tweet is considered **invalid** if its content length exceeds 15 characters.  
- We use the `LENGTH(content)` function to measure the number of characters.  
- Apply the condition `> 15` to filter invalid tweets.  
- Finally, select only the `tweet_id` of those tweets.
