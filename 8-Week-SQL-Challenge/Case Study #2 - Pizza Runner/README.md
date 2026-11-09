## Case Study 2: Pizza Runner

![Case Study 2](https://github.com/ShreyanshiJaiswal/8-Week-SQL-Challenge/blob/main/.images/Case_Study%232.png).

---

## Table of Contents

- [Business Task](#business-task)  
- [Entity Relationship Diagram (ERD)](#entity-relationship-diagram-erd)  
- Solution  
  - [Data Cleaning & Transformation](#data-cleaning--transformation)  
  - [Pizza Metrics](#pizza-metrics)  
  - [Runner and Customer Experience](#runner-and-customer-experience)  
  - [Ingredient Optimisation](#ingredient-optimisation)  
  - [Pricing and Ratings](#pricing-and-ratings)

Please note that all the information regarding this case study has been sourced from the following link: [here](https://8weeksqlchallenge.com/case-study-2/)

---

## Business Task

This case study focuses on helping Danny improve operations for his startup, Pizza Runner. Using data from the pizza_runner schema, the goal is to clean the raw datasets and perform basic analysis to support better decision-making around deliveries and orders. The tables involved in this case study include: runners, customer_orders, runner_orders, pizza_names, pizza_recipes, pizza_toppings, and members. All SQL queries should reference the pizza_runner schema.

---

## Entity Relationship Diagram (ERD)

![ERD](https://github.com/ShreyanshiJaiswal/8-Week-SQL-Challenge/blob/main/.images/ERD_Case%232.png)

---

## Data Cleaning & Transformation

### Table: customer_orders

Looking at the **customer_orders** table, we can see there are:  
- The `exclusions` column has missing, blank, or `'null'` values.
- The `extras` column has missing, blank, or `'null'` values.
  
| order_id | customer_id | pizza_id | exclusions | extras | order_time          |
| -------- | ----------- | -------- | ---------- | ------ | ------------------- |
| 1        | 101         | 1        |            |        | 2020-01-01 18:05:02 |
| 2        | 101         | 1        |            |        | 2020-01-01 19:00:52 |
| 3        | 102         | 1        |            |        | 2020-01-02 23:51:23 |
| 3        | 102         | 2        |            |        | 2020-01-02 23:51:23 |
| 4        | 103         | 1        | 4          |        | 2020-01-04 13:23:46 |
| 4        | 103         | 1        | 4          |        | 2020-01-04 13:23:46 |
| 4        | 103         | 2        | 4          |        | 2020-01-04 13:23:46 |
| 5        | 104         | 1        | null       | 1      | 2020-01-08 21:00:29 |
| 6        | 101         | 2        | null       | null   | 2020-01-08 21:03:13 |
| 7        | 105         | 2        | null       | 1      | 2020-01-08 21:20:29 |
| 8        | 102         | 1        | null       | null   | 2020-01-09 23:54:33 |
| 9        | 103         | 1        | 4          | 1, 5   | 2020-01-10 11:22:59 |
| 10       | 104         | 1        | null       | null   | 2020-01-11 18:34:49 |
| 10       | 104         | 1        | 2, 6       | 1, 4   | 2020-01-11 18:34:49 |

**Our work now:**  
- Replace all `NULL`, `'null'`, or blank entries in `exclusions` and `extras` columns with a blank space `' '`.  
- Create a temporary table with all the columns using this cleaned data.
```sql
CREATE TEMP TABLE customer_orders_temp AS
SELECT 
  order_id, 
  customer_id, 
  pizza_id, 
  CASE
	  WHEN exclusions IS null OR exclusions LIKE 'null' THEN ' '
	  ELSE exclusions
	  END AS exclusions,
  CASE
	  WHEN extras IS NULL or extras LIKE 'null' THEN ' '
	  ELSE extras
	  END AS extras,
	order_time
FROM pizza_runner.customer_orders;
```
This is how the clean customer_orders_temp table looks like and we will use this table to run all our queries.

![customer_orders_temp](https://github.com/ShreyanshiJaiswal/8-Week-SQL-Challenge/blob/main/.images/Case_Study%232_customer_orders_tem_table.png)

---

## Solution

---

## A. Pizza Metrics

### 1. How many pizzas were ordered?

```sql
SELECT
    COUNT(*) AS pizza_order_count
FROM customer_orders_temp;
```

**Output:**

| pizza_order_count |
|---:|
| 14 |

**Answer:** A total of 14 pizzas were ordered.

### 2. How many unique customer orders were made?

```sql
SELECT
    COUNT(DISTINCT order_id) AS unique_order_count
FROM customer_orders_temp;
```

**Output:**

| unique_order_count |
|---:|
| 10 |

**Answer:** There were 10 unique customer orders.

### 3. How many successful orders were delivered by each runner?

```sql
SELECT
    runner_id,
    COUNT(order_id) AS successful_orders
FROM runner_orders_temp
WHERE distance > 0
GROUP BY runner_id
ORDER BY runner_id;
```

**Output:**

| runner_id | successful_orders |
|---:|---:|
| 1 | 4 |
| 2 | 3 |
| 3 | 1 |

**Answer:**
- Runner 1 delivered 4 orders.
- Runner 2 delivered 3 orders.
- Runner 3 delivered 1 order.

### 4. How many of each type of pizza was delivered?

```sql
SELECT
    pizza_names.pizza_name,
    COUNT(customer_orders_temp.pizza_id) AS delivered_pizza_count
FROM customer_orders_temp
JOIN runner_orders_temp
    ON customer_orders_temp.order_id = runner_orders_temp.order_id
JOIN pizza_runner.pizza_names
    ON customer_orders_temp.pizza_id = pizza_names.pizza_id
WHERE runner_orders_temp.distance > 0
GROUP BY pizza_names.pizza_name;
```

**Output:**

| pizza_name | delivered_pizza_count |
|---|---:|
| Meatlovers | 9 |
| Vegetarian | 3 |

**Answer:** 9 Meatlovers and 3 Vegetarian pizzas were delivered.

### 5. How many Vegetarian and Meatlovers were ordered by each customer?

```sql
SELECT
    customer_orders_temp.customer_id,
    pizza_names.pizza_name,
    COUNT(pizza_names.pizza_name) AS order_count
FROM customer_orders_temp
JOIN pizza_runner.pizza_names
    ON customer_orders_temp.pizza_id = pizza_names.pizza_id
GROUP BY
    customer_orders_temp.customer_id,
    pizza_names.pizza_name
ORDER BY customer_orders_temp.customer_id;
```

**Output:**

| customer_id | pizza_name | order_count |
|---:|---|---:|
| 101 | Meatlovers | 2 |
| 101 | Vegetarian | 1 |
| 102 | Meatlovers | 2 |
| 102 | Vegetarian | 2 |
| 103 | Meatlovers | 3 |
| 103 | Vegetarian | 1 |
| 104 | Meatlovers | 1 |
| 105 | Vegetarian | 1 |

**Answer:**
- Customer 101 ordered 2 Meatlovers and 1 Vegetarian pizza.
- Customer 102 ordered 2 Meatlovers and 2 Vegetarian pizzas.
- Customer 103 ordered 3 Meatlovers and 1 Vegetarian pizza.
- Customer 104 ordered 1 Meatlovers pizza.
- Customer 105 ordered 1 Vegetarian pizza.

### 6. What was the maximum number of pizzas delivered in a single order?

```sql
WITH pizza_count_cte AS (
    SELECT
        customer_orders_temp.order_id,
        COUNT(customer_orders_temp.pizza_id) AS pizza_per_order
    FROM customer_orders_temp
    JOIN runner_orders_temp
        ON customer_orders_temp.order_id = runner_orders_temp.order_id
    WHERE runner_orders_temp.distance > 0
    GROUP BY customer_orders_temp.order_id
)
SELECT
    MAX(pizza_per_order) AS pizza_count
FROM pizza_count_cte;
```

**Output:**

| pizza_count |
|---:|
| 3 |

**Answer:** The maximum number of pizzas delivered in a single order was 3.

### 7. For each customer, how many delivered pizzas had at least 1 change and how many had no changes?

```sql
SELECT
    customer_orders_temp.customer_id,
    SUM(
        CASE
            WHEN customer_orders_temp.exclusions <> ' '
              OR customer_orders_temp.extras <> ' '
            THEN 1
            ELSE 0
        END
    ) AS at_least_1_change,
    SUM(
        CASE
            WHEN customer_orders_temp.exclusions = ' '
             AND customer_orders_temp.extras = ' '
            THEN 1
            ELSE 0
        END
    ) AS no_change
FROM customer_orders_temp
JOIN runner_orders_temp
    ON customer_orders_temp.order_id = runner_orders_temp.order_id
WHERE runner_orders_temp.distance > 0
GROUP BY customer_orders_temp.customer_id
ORDER BY customer_orders_temp.customer_id;
```

**Output:**

| customer_id | at_least_1_change | no_change |
|---:|---:|---:|
| 101 | 0 | 2 |
| 102 | 0 | 3 |
| 103 | 3 | 0 |
| 104 | 2 | 0 |
| 105 | 1 | 0 |

**Answer:**
- Customers 101 and 102 ordered pizzas without changes.
- Customers 103, 104 and 105 requested at least one change.

### 8. How many pizzas were delivered that had both exclusions and extras?

```sql
SELECT
    COUNT(*) AS pizza_count_w_exclusions_extras
FROM customer_orders_temp
JOIN runner_orders_temp
    ON customer_orders_temp.order_id = runner_orders_temp.order_id
WHERE runner_orders_temp.distance > 0
    AND customer_orders_temp.exclusions <> ' '
    AND customer_orders_temp.extras <> ' ';
```

**Output:**

| pizza_count_w_exclusions_extras |
|---:|
| 1 |

**Answer:** Only 1 delivered pizza had both exclusions and extras.

### 9. What was the total volume of pizzas ordered for each hour of the day?

```sql
SELECT
    EXTRACT(HOUR FROM order_time) AS hour_of_day,
    COUNT(order_id) AS pizza_count
FROM customer_orders_temp
GROUP BY EXTRACT(HOUR FROM order_time)
ORDER BY hour_of_day;
```

**Output:**

| hour_of_day | pizza_count |
|---:|---:|
| 11 | 1 |
| 13 | 3 |
| 18 | 3 |
| 19 | 1 |
| 21 | 3 |
| 23 | 1 |

**Answer:**
- Highest volume was at 1 PM, 6 PM and 9 PM.
- Lowest volume was at 11 AM, 7 PM and 11 PM.

### 10. What was the volume of orders for each day of the week?

```sql
SELECT
    TRIM(TO_CHAR(order_time, 'Day')) AS day_of_week,
    COUNT(order_id) AS total_pizzas_ordered
FROM customer_orders_temp
GROUP BY
    TRIM(TO_CHAR(order_time, 'Day')),
    EXTRACT(DOW FROM order_time)
ORDER BY EXTRACT(DOW FROM order_time);
```

**Output:**

| day_of_week | total_pizzas_ordered |
|---|---:|
| Sunday | 1 |
| Monday | 5 |
| Friday | 5 |
| Saturday | 3 |

**Answer:**
- 5 pizzas were ordered on Friday and Monday.
- 3 pizzas were ordered on Saturday.
- 1 pizza was ordered on Sunday.

## B. Runner and Customer Experience

### 1. How many runners signed up for each 1 week period? (i.e. week starts 2021-01-01)

```sql
SELECT
    EXTRACT(WEEK FROM registration_date) AS registration_week,
    COUNT(runner_id) AS runner_signup
FROM pizza_runner.runners
GROUP BY EXTRACT(WEEK FROM registration_date)
ORDER BY registration_week;
```

**Output:**

| registration_week | runner_signup |
|---:|---:|
| 1 | 2 |
| 2 | 1 |
| 3 | 1 |

**Answer:**
- 2 new runners signed up in Week 1 of January 2021.
- 1 new runner signed up in Week 2.
- 1 new runner signed up in Week 3.

### 2. What was the average time in minutes it took for each runner to arrive at the Pizza Runner HQ to pick up the order?

```sql
WITH time_taken_cte AS (
    SELECT
        customer_orders_temp.order_id,
        customer_orders_temp.order_time,
        runner_orders_temp.pickup_time,
        EXTRACT(EPOCH FROM (
            runner_orders_temp.pickup_time - customer_orders_temp.order_time
        )) / 60 AS pickup_minutes
    FROM customer_orders_temp
    JOIN runner_orders_temp
        ON customer_orders_temp.order_id = runner_orders_temp.order_id
    WHERE runner_orders_temp.distance > 0
)
SELECT
    ROUND(AVG(pickup_minutes), 0) AS avg_pickup_minutes
FROM time_taken_cte
WHERE pickup_minutes > 1;
```

**Output:**

| avg_pickup_minutes |
|---:|
| 15 |

**Answer:** On average, runners took 15 minutes to arrive at Pizza Runner HQ and pick up the orders.

### 3. Is there any relationship between the number of pizzas and how long the order takes to prepare?

```sql
WITH prep_time_cte AS (
    SELECT
        customer_orders_temp.order_id,
        COUNT(customer_orders_temp.order_id) AS pizza_order,
        customer_orders_temp.order_time,
        runner_orders_temp.pickup_time,
        EXTRACT(EPOCH FROM (
            runner_orders_temp.pickup_time - customer_orders_temp.order_time
        )) / 60 AS prep_time_minutes
    FROM customer_orders_temp
    JOIN runner_orders_temp
        ON customer_orders_temp.order_id = runner_orders_temp.order_id
    WHERE runner_orders_temp.distance > 0
    GROUP BY
        customer_orders_temp.order_id,
        customer_orders_temp.order_time,
        runner_orders_temp.pickup_time
)
SELECT
    pizza_order,
    ROUND(AVG(prep_time_minutes), 0) AS avg_prep_time_minutes
FROM prep_time_cte
WHERE prep_time_minutes > 1
GROUP BY pizza_order
ORDER BY pizza_order;
```

**Output:**

| pizza_order | avg_prep_time_minutes |
|---:|---:|
| 1 | 12 |
| 2 | 16 |
| 3 | 30 |

**Answer:**
- A single pizza order takes an average of 12 minutes to prepare.
- An order with 2 pizzas takes 16 minutes.
- An order with 3 pizzas takes 30 minutes.
- Preparation time generally increases with the number of pizzas ordered.

### 4. What was the average distance travelled for each customer?

```sql
SELECT
    customer_orders_temp.customer_id,
    AVG(runner_orders_temp.distance) AS avg_distance
FROM customer_orders_temp
JOIN runner_orders_temp
    ON customer_orders_temp.order_id = runner_orders_temp.order_id
WHERE runner_orders_temp.distance > 0
GROUP BY customer_orders_temp.customer_id
ORDER BY customer_orders_temp.customer_id;
```

**Output:**

| customer_id | avg_distance |
|---:|---:|
| 101 | 20 |
| 102 | 16 |
| 103 | 23.4 |
| 104 | 10 |
| 105 | 25 |

**Answer:** Assuming distance is measured from Pizza Runner HQ to the customer's location:
- Customer 104 is the nearest, at an average distance of 10 km.
- Customer 105 is the farthest, at an average distance of 25 km.

### 5. What was the difference between the longest and shortest delivery times for all orders?

```sql
SELECT
    MAX(duration) - MIN(duration) AS delivery_time_difference
FROM runner_orders_temp
WHERE duration IS NOT NULL;
```

**Output:**

| delivery_time_difference |
|---:|
| 30 |

**Answer:** The difference between the longest delivery time (40 minutes) and shortest delivery time (10 minutes) is 30 minutes.

### 6. What was the average speed for each runner for each delivery and do you notice any trend for these values?

```sql
SELECT
    runner_orders_temp.runner_id,
    customer_orders_temp.customer_id,
    customer_orders_temp.order_id,
    COUNT(customer_orders_temp.order_id) AS pizza_count,
    runner_orders_temp.distance,
    runner_orders_temp.duration / 60.0 AS duration_hr,
    ROUND(
        runner_orders_temp.distance / runner_orders_temp.duration * 60,
        2
    ) AS avg_speed
FROM runner_orders_temp
JOIN customer_orders_temp
    ON runner_orders_temp.order_id = customer_orders_temp.order_id
WHERE runner_orders_temp.distance > 0
GROUP BY
    runner_orders_temp.runner_id,
    customer_orders_temp.customer_id,
    customer_orders_temp.order_id,
    runner_orders_temp.distance,
    runner_orders_temp.duration
ORDER BY customer_orders_temp.order_id;
```

**Output:**

| runner_id | customer_id | order_id | pizza_count | distance | duration_hr | avg_speed |
|---:|---:|---:|---:|---:|---:|---:|
| 1 | 101 | 1 | 1 | 20 | 0.3333 | 60 |
| 1 | 101 | 2 | 2 | 20 | 0.3333 | 60 |
| 1 | 102 | 3 | 1 | 13.4 | 0.2167 | 61.85 |
| 2 | 103 | 4 | 3 | 23.4 | 0.4167 | 56.16 |
| 3 | 104 | 5 | 1 | 10 | 0.25 | 40 |
| 2 | 105 | 7 | 1 | 25 | 0.4167 | 60 |
| 2 | 102 | 8 | 1 | 23.4 | 0.25 | 93.6 |
| 1 | 104 | 10 | 2 | 10 | 0.1667 | 60 |

**Answer:**
- Runner 1's average speed ranges from 37.5 km/h to 60 km/h.
- Runner 2's average speed ranges from 35.1 km/h to 93.6 km/h.
- Runner 3's average speed is 40 km/h.
- Runner 2 has the highest variation in delivery speed.

*Average speed = Distance in km / Duration in hours.*

### 7. What is the successful delivery percentage for each runner?

```sql
SELECT
    runner_id,
    ROUND(
        100.0 * SUM(
            CASE
                WHEN distance > 0 THEN 1
                ELSE 0
            END
        ) / COUNT(*),
        0
    ) AS success_perc
FROM runner_orders_temp
GROUP BY runner_id
ORDER BY runner_id;
```

**Output:**

| runner_id | success_perc |
|---:|---:|
| 1 | 100 |
| 2 | 75 |
| 3 | 50 |

**Answer:**
- Runner 1 has a 100% successful delivery rate.
- Runner 2 has a 75% successful delivery rate.
- Runner 3 has a 50% successful delivery rate.

*Note: Order cancellations may be outside the runners' control, so these percentages should not be treated as a direct measure of runner performance.*
