# Case Study #3 - Foodie-Fi

![Case Study 3](https://github.com/ShreyanshiJaiswal/Learning_Log/blob/main/8-Week-SQL-Challenge/.images/Case_Study%233.png)

## Table of Contents

- [Business Task](#business-task)
- [Entity Relationship Diagram](#entity-relationship-diagram)
- [Questions and Solutions](#questions-and-solutions)

  Please note that all the information regarding this case study has been sourced from the following link: [here](https://8weeksqlchallenge.com/case-study-3/).

## Business Task

Danny and his friends launched a new startup Foodie-Fi and started selling monthly and annual subscriptions, giving their customers unlimited on-demand access to exclusive food videos from around the world.

This case study focuses on using subscription-style digital data to answer important business questions on customer journey, payments, and business performance.

## Entity Relationship Diagram

![Entity Relationship Diagram](https://github.com/ShreyanshiJaiswal/Learning_Log/blob/main/8-Week-SQL-Challenge/.images/ERD_Case%233.png)

### Table 1: `plans`

![Plans Table](https://github.com/ShreyanshiJaiswal/Learning_Log/blob/main/8-Week-SQL-Challenge/.images/Case_Study%233_plans_table.png)

There are 5 customer plans.

- **Trial:** Customers sign up for an initial 7-day free trial and automatically continue with the Pro Monthly subscription unless they cancel, downgrade to Basic, or upgrade to an Annual Pro plan during the trial.
- **Basic plan:** Customers have limited access and can only stream videos. It is available monthly at $9.90.
- **Pro plan:** Customers have no watch-time limits and can download videos for offline viewing. Pro plans cost $19.90 per month or $199 for an annual subscription.

When customers cancel their Foodie-Fi service, they receive a Churn plan record with a NULL price. However, their subscription continues until the end of the billing period.

### Table 2: `subscriptions`

![Subscriptions Table](https://github.com/ShreyanshiJaiswal/Learning_Log/blob/main/8-Week-SQL-Challenge/.images/Case_Study%233_subscriptions_table.png)

Customer subscriptions show the exact date when their specific `plan_id` starts.

- If customers downgrade from a Pro plan or cancel their subscription, the higher plan remains active until the current billing period ends. The `start_date` in the subscriptions table reflects the date when the actual plan changes.
- When customers upgrade from a Basic plan to a Pro or Annual Pro plan, the higher plan takes effect immediately.
- When customers churn, they retain access until the end of their current billing period. However, the `start_date` records the date they decided to cancel.

## Questions and Solutions

### A. Customer Journey

**Question:** Based on the 8 sample customers provided in the `subscriptions` table, write a brief description about each customer's onboarding journey.

```sql
SELECT
    subscriptions.customer_id,
    plans.plan_id,
    plans.plan_name,
    subscriptions.start_date
FROM foodie_fi.subscriptions
JOIN foodie_fi.plans
    ON subscriptions.plan_id = plans.plan_id
WHERE subscriptions.customer_id IN (1, 2, 11, 13, 15, 16, 18, 19)
ORDER BY subscriptions.customer_id, subscriptions.start_date;
```

**Output:**

| customer_id | plan_id | plan_name | start_date |
|---:|---:|---|---|
| 1 | 0 | trial | 2020-08-01 |
| 1 | 1 | basic monthly | 2020-08-08 |
| 2 | 0 | trial | 2020-09-20 |
| 2 | 3 | pro annual | 2020-09-27 |
| 11 | 0 | trial | 2020-11-19 |
| 11 | 4 | churn | 2020-11-26 |
| 13 | 0 | trial | 2020-12-15 |
| 13 | 1 | basic monthly | 2020-12-22 |
| 13 | 2 | pro monthly | 2021-03-29 |
| 15 | 0 | trial | 2020-03-17 |
| 15 | 2 | pro monthly | 2020-03-24 |
| 15 | 4 | churn | 2020-04-29 |
| 16 | 0 | trial | 2020-05-31 |
| 16 | 1 | basic monthly | 2020-06-07 |
| 16 | 3 | pro annual | 2020-10-21 |
| 18 | 0 | trial | 2020-07-06 |
| 18 | 2 | pro monthly | 2020-07-13 |
| 19 | 0 | trial | 2020-06-22 |
| 19 | 2 | pro monthly | 2020-06-29 |
| 19 | 3 | pro annual | 2020-08-29 |

**Customer Journey:**

- **Customer 1:** Started with a free trial on 1 August 2020 and subscribed to the Basic monthly plan on 8 August 2020.
- **Customer 13:** Started with a free trial on 15 December 2020, subscribed to the Basic monthly plan on 22 December 2020, and upgraded to the Pro monthly plan on 29 March 2021.
- **Customer 15:** Started with a free trial on 17 March 2020, upgraded to the Pro monthly plan on 24 March 2020, and decided to cancel on 29 April 2020.

## B. Data Analysis Questions

### 1. How many customers has Foodie-Fi ever had?

```sql
SELECT
    COUNT(DISTINCT customer_id) AS total_customers
FROM foodie_fi.subscriptions;
```

**Output:**

| total_customers |
|---:|
| 1000 |

**Answer:** Foodie-Fi has 1,000 unique customers.

### 2. What is the monthly distribution of `trial` plan `start_date` values for our dataset - use the start of the month as the group by value?

```sql
SELECT
    DATE_TRUNC('month', start_date)::DATE AS month_start,
    COUNT(customer_id) AS customer_count
FROM foodie_fi.subscriptions
WHERE plan_id = 0
GROUP BY DATE_TRUNC('month', start_date)
ORDER BY month_start;
```

**Output:**

| month_start | customer_count |
|---|---:|
| 2020-01-01 | 88 |
| 2020-02-01 | 68 |
| 2020-03-01 | 94 |
| 2020-04-01 | 81 |
| 2020-05-01 | 88 |
| 2020-06-01 | 79 |
| 2020-07-01 | 89 |
| 2020-08-01 | 88 |
| 2020-09-01 | 87 |
| 2020-10-01 | 79 |
| 2020-11-01 | 75 |
| 2020-12-01 | 84 |

**Answer:**
- March had the highest number of trial sign-ups, with 94 customers.
- February had the lowest number, with 68 customers.

### 3. What plan `start_date` values occur after the year 2020 for our dataset? Show the breakdown by count of events for each `plan_name`.

```sql
SELECT
    plans.plan_name,
    COUNT(subscriptions.customer_id) AS customer_count
FROM foodie_fi.subscriptions
JOIN foodie_fi.plans
    ON subscriptions.plan_id = plans.plan_id
WHERE subscriptions.start_date >= '2021-01-01'
GROUP BY plans.plan_name
ORDER BY plans.plan_name;
```

**Output:**

| plan_name | customer_count |
|---|---:|
| basic monthly | 8 |
| churn | 71 |
| pro annual | 63 |
| pro monthly | 60 |

**Answer:**
- 71 churn events occurred after 2020.
- Pro Annual had 63 events.
- Pro Monthly had 60 events.
- Basic Monthly had 8 events.

### 4. What is the customer count and percentage of customers who have churned rounded to 1 decimal place?

```sql
SELECT
    COUNT(DISTINCT subscriptions.customer_id) AS churned_customers,
    ROUND(
        100.0 * COUNT(DISTINCT subscriptions.customer_id) /
        (SELECT COUNT(DISTINCT customer_id)
         FROM foodie_fi.subscriptions),
        1
    ) AS churn_percentage
FROM foodie_fi.subscriptions
WHERE plan_id = 4;
```

**Output:**

| churned_customers | churn_percentage |
|---:|---:|
| 307 | 30.7 |

**Answer:** 307 customers have churned, representing 30.7% of the total customer base.

### 5. How many customers have churned straight after their initial free trial - what percentage is this rounded to the nearest whole number?

```sql
WITH next_plan_cte AS (
    SELECT
        customer_id,
        plan_id,
        LEAD(plan_id) OVER (
            PARTITION BY customer_id
            ORDER BY start_date
        ) AS next_plan
    FROM foodie_fi.subscriptions
)
SELECT
    COUNT(customer_id) AS churned_customers,
    ROUND(
        100.0 * COUNT(customer_id) /
        (SELECT COUNT(DISTINCT customer_id)
         FROM foodie_fi.subscriptions),
        0
    ) AS churn_percentage
FROM next_plan_cte
WHERE plan_id = 0
    AND next_plan = 4;
```

**Output:**

| churned_customers | churn_percentage |
|---:|---:|
| 92 | 9 |

**Answer:** 92 customers churned immediately after their free trial, representing 9% of all customers.

### 6. What is the number and percentage of customer plans after their initial free trial?

```sql
WITH next_plan_cte AS (
    SELECT
        subscriptions.customer_id,
        subscriptions.plan_id,
        plans.plan_name,
        LEAD(subscriptions.plan_id) OVER (
            PARTITION BY subscriptions.customer_id
            ORDER BY subscriptions.start_date
        ) AS next_plan
    FROM foodie_fi.subscriptions
    JOIN foodie_fi.plans
        ON subscriptions.plan_id = plans.plan_id
)
SELECT
    plans.plan_name,
    COUNT(next_plan_cte.customer_id) AS customer_count,
    ROUND(
        100.0 * COUNT(next_plan_cte.customer_id) /
        (SELECT COUNT(DISTINCT customer_id)
         FROM foodie_fi.subscriptions),
        1
    ) AS percentage
FROM next_plan_cte
JOIN foodie_fi.plans
    ON next_plan_cte.next_plan = plans.plan_id
WHERE next_plan_cte.plan_id = 0
GROUP BY plans.plan_name
ORDER BY plans.plan_name;
```

**Output:**

| plan_name | customer_count | percentage |
|---|---:|---:|
| basic monthly | 546 | 54.6 |
| churn | 92 | 9.2 |
| pro annual | 37 | 3.7 |
| pro monthly | 325 | 32.5 |

**Answer:**
- 546 customers moved to the Basic Monthly plan.
- 325 customers moved to the Pro Monthly plan.
- 37 customers moved to the Pro Annual plan.
- 92 customers churned after their trial.

### 7. What is the customer count and percentage breakdown of all 5 `plan_name` values at `2020-12-31`?

```sql
WITH customer_plans AS (
    SELECT
        subscriptions.customer_id,
        subscriptions.plan_id,
        subscriptions.start_date,
        LEAD(subscriptions.start_date) OVER (
            PARTITION BY subscriptions.customer_id
            ORDER BY subscriptions.start_date
        ) AS next_date
    FROM foodie_fi.subscriptions
)
SELECT
    plans.plan_name,
    COUNT(customer_plans.customer_id) AS customer_count,
    ROUND(
        100.0 * COUNT(customer_plans.customer_id) /
        (SELECT COUNT(DISTINCT customer_id)
         FROM foodie_fi.subscriptions),
        1
    ) AS customer_percentage
FROM customer_plans
JOIN foodie_fi.plans
    ON customer_plans.plan_id = plans.plan_id
WHERE customer_plans.start_date <= '2020-12-31'
    AND (
        customer_plans.next_date > '2020-12-31'
        OR customer_plans.next_date IS NULL
    )
GROUP BY plans.plan_name
ORDER BY plans.plan_name;
```

**Output:**

| plan_name | customer_count | customer_percentage |
|---|---:|---:|
| basic monthly | 224 | 22.4 |
| churn | 235 | 23.5 |
| pro annual | 195 | 19.5 |
| pro monthly | 326 | 32.6 |
| trial | 19 | 1.9 |

**Answer:**
- Pro Monthly has the highest customer share at 32.6%.
- 22.4% of customers are on the Basic Monthly plan.
- 19.5% are on the Pro Annual plan.
- 23.5% have a Churn plan record.
- 1.9% are still on a Trial plan.

### 8. How many customers have upgraded to an annual plan in 2020?

```sql
SELECT
    COUNT(customer_id) AS annual_plan_customers
FROM foodie_fi.subscriptions
WHERE plan_id = 3
    AND start_date >= '2020-01-01'
    AND start_date < '2021-01-01';
```

**Output:**

| annual_plan_customers |
|---:|
| 195 |

**Answer:** 195 customers upgraded to the Pro Annual plan in 2020.

### 9. How many days on average does it take for a customer to an annual plan from the day they join Foodie-Fi?

```sql
WITH trial_plan AS (
    SELECT
        customer_id,
        start_date AS trial_date
    FROM foodie_fi.subscriptions
    WHERE plan_id = 0
),
annual_plan AS (
    SELECT
        customer_id,
        start_date AS annual_date
    FROM foodie_fi.subscriptions
    WHERE plan_id = 3
)
SELECT
    ROUND(
        AVG(annual_plan.annual_date - trial_plan.trial_date)
    ) AS avg_days_to_upgrade
FROM trial_plan
JOIN annual_plan
    ON trial_plan.customer_id = annual_plan.customer_id;
```

**Output:**

| avg_days_to_upgrade |
|---:|
| 105 |

**Answer:** On average, it takes 105 days for a customer to upgrade to the Pro Annual plan from their initial trial date.

### 10. Can you further breakdown this average value into 30 day periods (i.e. 0-30 days, 31-60 days etc)?

```sql
WITH trial_plan AS (
    SELECT
        customer_id,
        start_date AS trial_date
    FROM foodie_fi.subscriptions
    WHERE plan_id = 0
),
annual_plan AS (
    SELECT
        customer_id,
        start_date AS annual_date
    FROM foodie_fi.subscriptions
    WHERE plan_id = 3
),
upgrade_days AS (
    SELECT
        annual_plan.customer_id,
        annual_plan.annual_date - trial_plan.trial_date AS days_to_upgrade
    FROM trial_plan
    JOIN annual_plan
        ON trial_plan.customer_id = annual_plan.customer_id
),
upgrade_periods AS (
    SELECT
        CASE
            WHEN days_to_upgrade <= 30 THEN '0-30 days'
            WHEN days_to_upgrade <= 60 THEN '31-60 days'
            WHEN days_to_upgrade <= 90 THEN '61-90 days'
            WHEN days_to_upgrade <= 120 THEN '91-120 days'
            WHEN days_to_upgrade <= 150 THEN '121-150 days'
            WHEN days_to_upgrade <= 180 THEN '151-180 days'
            WHEN days_to_upgrade <= 210 THEN '181-210 days'
            WHEN days_to_upgrade <= 240 THEN '211-240 days'
            WHEN days_to_upgrade <= 270 THEN '241-270 days'
            WHEN days_to_upgrade <= 300 THEN '271-300 days'
            WHEN days_to_upgrade <= 330 THEN '301-330 days'
            WHEN days_to_upgrade <= 360 THEN '331-360 days'
        END AS day_period,
        CASE
            WHEN days_to_upgrade <= 30 THEN 1
            WHEN days_to_upgrade <= 60 THEN 2
            WHEN days_to_upgrade <= 90 THEN 3
            WHEN days_to_upgrade <= 120 THEN 4
            WHEN days_to_upgrade <= 150 THEN 5
            WHEN days_to_upgrade <= 180 THEN 6
            WHEN days_to_upgrade <= 210 THEN 7
            WHEN days_to_upgrade <= 240 THEN 8
            WHEN days_to_upgrade <= 270 THEN 9
            WHEN days_to_upgrade <= 300 THEN 10
            WHEN days_to_upgrade <= 330 THEN 11
            WHEN days_to_upgrade <= 360 THEN 12
        END AS period_order
    FROM upgrade_days
)
SELECT
    day_period,
    COUNT(*) AS customer_count
FROM upgrade_periods
GROUP BY day_period, period_order
ORDER BY period_order;
```

**Output:**

| day_period | customer_count |
|---|---:|
| 0-30 days | 48 |
| 31-60 days | 25 |
| 61-90 days | 33 |
| 91-120 days | 35 |
| 121-150 days | 43 |
| 151-180 days | 35 |
| 181-210 days | 27 |
| 211-240 days | 4 |
| 241-270 days | 5 |
| 271-300 days | 1 |
| 301-330 days | 1 |
| 331-360 days | 1 |

**Answer:**
- 48 customers upgraded within the first 30 days.
- Most customers upgraded within the first 180 days.
- Very few customers took more than 240 days to upgrade.

### 11. How many customers downgraded from a pro monthly to a basic monthly plan in 2020?

```sql
WITH next_plan_cte AS (
    SELECT
        customer_id,
        plan_id,
        start_date,
        LEAD(plan_id) OVER (
            PARTITION BY customer_id
            ORDER BY start_date
        ) AS next_plan
    FROM foodie_fi.subscriptions
)
SELECT
    COUNT(DISTINCT customer_id) AS downgraded_customers
FROM next_plan_cte
WHERE plan_id = 2
    AND next_plan = 1
    AND start_date >= '2020-01-01'
    AND start_date < '2021-01-01';
```

**Output:**

| downgraded_customers |
|---:|
| 0 |

**Answer:** No customers downgraded from the Pro Monthly plan to the Basic Monthly plan in 2020.
