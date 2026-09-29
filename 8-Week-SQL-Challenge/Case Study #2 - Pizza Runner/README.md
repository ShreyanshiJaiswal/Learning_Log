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

**1. How many pizzas were ordered?**
```sql

```
