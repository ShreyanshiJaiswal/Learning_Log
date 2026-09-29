# Guesstimate 11: Number of Weddings in India in a Year

## Clarifying Questions

1. Are we considering the entire population of India?
2. Are we considering all religions and regions?
3. Are we including both urban and rural India?
4. Are we counting first marriages as well as remarriages?
5. Are we including both registered and unregistered marriages?
6. Are community/group weddings counted separately for each couple?
7. Are we estimating weddings in a typical year?

## Final Question

**How many weddings take place in India in a typical year, including all religions, regions, urban and rural areas, first marriages, remarriages, and both registered and unregistered marriages?**

---

# Solution

## Key Logic

We need to estimate the **annual number of people entering marriage** and convert that into the number of weddings.

The main marriage-age population is divided into two groups because the likelihood of marriage is different across ages:

**20–29 → larger unmarried population → longer marriage conversion period**

**30–39 → smaller unmarried population → shorter marriage conversion period**

After estimating first marriages from these two groups, we account for:

1. **First marriages outside the 20–39 age group**
2. **Remarriages at any age**

We do not separately divide remarriages by age because remarriage can happen across different age groups and doing so would add unnecessary assumptions.

---

## Step 1: Estimate India's Population by Age Group

Assume India's population is approximately:

**1.4 billion**

Assume:

- **20–29 = 18%**
- **30–39 = 15%**

Therefore:

| Age Group | Population Share | Population |
|---|---:|---:|
| 20–29 | 18% | 252M |
| 30–39 | 15% | 210M |

### 20–29 Population

**1.4B × 18% = 252M**

### 30–39 Population

**1.4B × 15% = 210M**

---

## Step 2: Estimate the Unmarried Population

### Age 20–29

Assume approximately **50%** of people aged 20–29 are unmarried.

Reason:

The early 20s contain many unmarried people, while marriage becomes more common toward the late 20s. Therefore, taking roughly half of this age group as unmarried is a reasonable simplifying assumption.

**252M × 50% = 126M**

Therefore:

**Unmarried 20–29 population ≈ 126M**

---

### Age 30–39

Assume approximately **10%** of people aged 30–39 are unmarried.

Reason:

By the 30s, the majority of people who intend to marry have already married, so the unmarried proportion should be significantly lower than in the 20–29 group.

**210M × 10% = 21M**

Therefore:

**Unmarried 30–39 population ≈ 21M**

---

## Step 3: Estimate Annual First Marriages

### Age 20–29

There are approximately:

**126M unmarried people**

Assume they transition into marriage over approximately **8 years**.

Reason:

The group covers ages 20–29, and people do not all marry at the same age. Some marry in their early 20s, while others marry toward their late 20s.

Therefore:

**126M ÷ 8 = 15.75M people/year**

---

### Age 30–39

There are approximately:

**21M unmarried people**

Assume they transition into marriage over approximately **5 years**.

Reason:

People who remain unmarried into their 30s have a higher probability of marrying relatively soon than people in their early 20s. Therefore, a shorter conversion period is more appropriate.

**21M ÷ 5 = 4.2M people/year**

---

## Step 4: Convert People Into Weddings

Total people entering first marriage:

**15.75M + 4.2M = 19.95M people/year**

Each wedding involves approximately two people.

Therefore:

**19.95M ÷ 2 = 9.975M**

So:

**≈ 10M first marriages/weddings per year**

This is our core estimate.

---

## Step 5: Account for First Marriages Outside the 20–39 Age Group

The 20–39 age groups capture the majority of first marriages, but not all of them.

There will also be:

- First marriages before age 20
- First marriages after age 39

Assume approximately **5% of first marriages occur outside the 20–39 age range**.

Reason:

Most first marriages are expected to occur within the main marriage-age range, so marriages outside this range should form a relatively small minority.

If the 10M estimate represents approximately 95% of first marriages:

**Total first marriages = 10M ÷ 95%**

**≈ 10.5M first marriages/year**

---

## Step 6: Account for Remarriages

Remarriages are treated as a separate bucket and can occur at **any age**.

Assume approximately **2% of all weddings are remarriages**.

Reason:

Remarriage is relatively uncommon in India. Although remarriage can occur because of divorce or widowhood, a large majority of marriages remain first marriages. Therefore, we use a small 2% assumption rather than the earlier 5% assumption.

Since remarriages represent 2% of the final wedding pool:

**First marriages = 98% of total weddings**

Therefore:

**Total weddings = 10.5M ÷ 98%**

**≈ 10.7M**

---

# Final Estimate

## **≈ 11 million weddings per year in India**

Given the uncertainty in the population and marriage-rate assumptions, a reasonable range is:

**~9–12 million weddings per year**

## Summary

| Component | Estimate |
|---|---:|
| Population aged 20–29 | 252M |
| Unmarried 20–29 | 126M |
| Annual first marriages from 20–29 | 15.75M people |
| Population aged 30–39 | 210M |
| Unmarried 30–39 | 21M |
| Annual first marriages from 30–39 | 4.2M people |
| Total people entering first marriage | 19.95M |
| Core first weddings | 9.98M |
| First marriages outside 20–39 | +5% adjustment |
| Total first marriages | ~10.5M |
| Remarriages | 2% of total weddings |
| **Final estimate** | **~10.7M ≈ 11M weddings/year** |
