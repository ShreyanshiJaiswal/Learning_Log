## Case Study 1: Danny's Diner

![Case Study 1](https://raw.githubusercontent.com/ShreyanshiJaiswal/8-Week-SQL-Challenge/main/.images/Case_Study%231.png)

---

## Table of Contents

- [Business Task](#business-task)  
- [Entity Relationship Diagram (ERD)](#entity-relationship-diagram-erd)  
- [Question and Solution](#question-and-solution)  

Please note that all the information regarding this case study has been sourced from the following link: [here](https://8weeksqlchallenge.com/case-study-1/)

---

## Business Task

In this case study, I'm helping Danny analyze customer data to understand their visit patterns, spending habits, and favorite menu items. I’ll use SQL to answer his business questions using the `sales`, `menu`, and `members` datasets.

---

## Entity Relationship Diagram (ERD)

![ERD](https://raw.githubusercontent.com/ShreyanshiJaiswal/8-Week-SQL-Challenge/main/.images/ERD_Case%231.png)

---

## Question and Solution

I’ve solved the following question on [DB Fiddle](https://www.db-fiddle.com/f/2rM8RAnq7h5LLDTzZiRWcd/138) 💻

---

**1. What is the total amount each customer spent at the restaurant?**
```sql
SELECT 
  sales.customer_id, 
  SUM(menu.price) AS total_sales
FROM dannys_diner.sales
INNER JOIN dannys_diner.menu
  ON sales.product_id = menu.product_id
GROUP BY sales.customer_id
ORDER BY sales.customer_id ASC;
```
**Output:**
| customer\_id | total\_sales |
| ------------ | ------------ |
| A            | 76           |
| B            | 74           |
| C            | 36           |

**2. How many days has each customer visited the restaurant?**
```sql
SELECT 
  customer_id, 
  COUNT(DISTINCT order_date) AS visit_count
FROM dannys_diner.sales
GROUP BY customer_id;
```
**Output:**
| customer_id | visit_count |
|-------------|-------------|
| A           | 4           |
| B           | 6           |
| C           | 2           |

**3. What was the first item from the menu purchased by each customer?**
```sql
SELECT 
  s.customer_id, 
  s.order_date, 
  m.product_name
FROM dannys_diner.sales AS s
JOIN dannys_diner.menu AS m 
  ON s.product_id = m.product_id
WHERE s.order_date = (
  SELECT MIN(order_date)
  FROM dannys_diner.sales AS inner_s
  WHERE inner_s.customer_id = s.customer_id
)
ORDER BY s.customer_id;
```
**Output:**
| customer_id | order_date | product_name |
|-------------|------------|--------------|
| A           | 2021-01-01 | sushi        |
| A           | 2021-01-01 | curry        |
| B           | 2021-01-01 | curry        |
| C           | 2021-01-01 | ramen        |

**4. Find the most purchased item on the menu and how many times it was purchased?**
```sql
SELECT 
  menu.product_name, 
  COUNT(*) AS frequency
FROM dannys_diner.sales
JOIN dannys_diner.menu 
  ON sales.product_id = menu.product_id
GROUP BY menu.product_name
ORDER BY frequency DESC
LIMIT 1;
```
**Output:**
| product_name | frequency |
|--------------|-----------|
| ramen        | 8         |

**5. Which item was the most popular for each customer?**
```sql
WITH ranked_orders AS (
  SELECT 
    dannys_diner.sales.customer_id,
    dannys_diner.menu.product_name,
    COUNT(*) AS order_count,
    RANK() OVER (
      PARTITION BY dannys_diner.sales.customer_id 
      ORDER BY COUNT(*) DESC
    ) AS rank
  FROM dannys_diner.sales
  JOIN dannys_diner.menu ON dannys_diner.sales.product_id = dannys_diner.menu.product_id
  GROUP BY dannys_diner.sales.customer_id, dannys_diner.menu.product_name
)

SELECT customer_id, product_name, order_count
FROM ranked_orders
WHERE rank = 1;

```
**Output:**
| customer\_id | product\_name | order\_count |
| ------------ | ------------- | ------------ |
| A            | Curry         | 2            |
| B            | Sushi         | 2            |
| C            | Ramen         | 3            |

**6. Which item was purchased first by the customer after they became a member?**
```sql
WITH joined_as_member AS (
  SELECT
    members.customer_id, 
    sales.product_id,
    ROW_NUMBER() OVER (
      PARTITION BY members.customer_id
      ORDER BY sales.order_date) AS row_num
  FROM dannys_diner.members
  INNER JOIN dannys_diner.sales
    ON members.customer_id = sales.customer_id
    AND sales.order_date > members.join_date
)

SELECT 
  customer_id, 
  product_name 
FROM joined_as_member
INNER JOIN dannys_diner.menu
  ON joined_as_member.product_id = menu.product_id
WHERE row_num = 1
ORDER BY customer_id ASC;
```
**Output**
| customer_id | product_name |
|-------------|--------------|
| A           | curry        |
| B           | sushi        |

**7. Which item was purchased just before the customer became a member?**
```sql
WITH purchased_prior_member AS (
  SELECT 
    members.customer_id, 
    sales.product_id,
    ROW_NUMBER() OVER (
      PARTITION BY members.customer_id
      ORDER BY sales.order_date DESC) AS rank
  FROM dannys_diner.members
  INNER JOIN dannys_diner.sales
    ON members.customer_id = sales.customer_id
    AND sales.order_date < members.join_date
)

SELECT 
  p_member.customer_id, 
  menu.product_name 
FROM purchased_prior_member AS p_member
INNER JOIN dannys_diner.menu
  ON p_member.product_id = menu.product_id
WHERE rank = 1
ORDER BY p_member.customer_id ASC;
```
**Output**
| customer_id | product_name |
|-------------|--------------|
| A           | sushi        |
| B           | sushi        |

**8. What is the total items and amount spent for each member before they became a member?**
```sql
SELECT 
  sales.customer_id, 
  COUNT(sales.product_id) AS total_items, 
  SUM(menu.price) AS total_sales
FROM dannys_diner.sales
INNER JOIN dannys_diner.members
  ON sales.customer_id = members.customer_id
  AND sales.order_date < members.join_date
INNER JOIN dannys_diner.menu
  ON sales.product_id = menu.product_id
GROUP BY sales.customer_id
ORDER BY sales.customer_id;
```
**Output**
| customer_id | total_items | total_sales |
|-------------|-------------|-------------|
| A           | 2           | 25          |
| B           | 3           | 40          |

**9. If each $1 spent equates to 10 points and sushi has a 2x points multiplier - how many points would each customer have?**
```sql
WITH points_cte AS (
  SELECT 
    menu.product_id, 
    CASE
      WHEN product_id = 1 THEN price * 20
      ELSE price * 10 
    END AS points
  FROM dannys_diner.menu
)

SELECT 
  sales.customer_id, 
  SUM(points_cte.points) AS total_points
FROM dannys_diner.sales
INNER JOIN points_cte
  ON sales.product_id = points_cte.product_id
GROUP BY sales.customer_id
ORDER BY sales.customer_id;
```
**Output**
| customer_id | total_points |
|-------------|--------------|
| A           | 760          |
| B           | 740          |
| C           | 360          |

**10. In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi - how many points do customer A and B have at the end of January?**
```sql
SELECT 
  sales.customer_id,
  SUM(
    CASE 
      WHEN sales.order_date BETWEEN members.join_date AND DATE_ADD(members.join_date, INTERVAL 6 DAY) THEN menu.price * 10 * 2
      ELSE menu.price * 10
    END
  ) AS total_points
FROM dannys_diner.sales
JOIN dannys_diner.menu
  ON sales.product_id = menu.product_id
JOIN dannys_diner.members
  ON sales.customer_id = members.customer_id
WHERE sales.customer_id IN ('A', 'B')
  AND sales.order_date <= '2021-01-31'
GROUP BY sales.customer_id
ORDER BY sales.customer_id;
```
**Output**
| customer_id | total_points |
|-------------|--------------|
| A           | 450          |
| B           | 500          |
