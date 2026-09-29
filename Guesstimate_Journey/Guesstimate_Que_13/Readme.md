# Guesstimate 13: How Many Flights Take Off in India in a Single Day?

## 1. Clarifying Questions

1. Are we estimating flights in India, a particular city, or the entire globe?
2. Should we consider both domestic and international flights?
3. Are we considering flights throughout the entire day or only during peak hours?
4. Should we include all airports, including small regional airports, or only major airports?
5. Are we counting only commercial passenger flights?

## 2. Final Question

> How many domestic and international commercial passenger flights take off from all airports across India over a normal 24-hour day?

## 3. Solution

We divide Indian airports into three broad categories based on their operational activity:

- Major airports
- Medium airports
- Small airports

For each category:

**Number of airports → Operating hours → Movement frequency → Daily movements → Takeoffs**

An aircraft movement means either a landing or a takeoff. Therefore, we assume approximately 50% of total movements are takeoffs.

## 4. Key Assumptions

| Airport Type | Number of Airports | Operating Hours/Day | Movement Frequency |
|---|---:|---:|---:|
| Major | 30 | 18 hours | 1 movement every 5 minutes |
| Medium | 70 | 16 hours | 1 movement every 10 minutes |
| Small | 40 | 10 hours | 1 movement every 30 minutes |

### Assumption Logic

- **Major airports:** High-volume airports with frequent aircraft operations. Assume 18 operating hours and one movement every 5 minutes.
- **Medium airports:** Moderate scheduled activity. Assume 16 operating hours and one movement every 10 minutes.
- **Small airports:** Limited regional operations. Assume 10 operating hours and one movement every 30 minutes.
- **Airport counts:** Assume 30 major, 70 medium, and 40 small airports to represent India's airport network across the three activity levels.

## 5. Step-by-Step Calculation

### Step 1: Major Airports

Number of major airports:

**30**

Operating time per airport:

**18 hours × 60 minutes = 1,080 minutes/day**

Movement frequency:

**1 movement every 5 minutes**

Movements per airport:

**1,080 ÷ 5 = 216 ≈ 200 movements/day**

Total major-airport movements:

**30 × 200 = 6,000 movements/day**

Takeoffs:

**6,000 × 50% = 3,000 takeoffs/day**

---

### Step 2: Medium Airports

Number of medium airports:

**70**

Operating time per airport:

**16 hours × 60 minutes = 960 minutes/day**

Movement frequency:

**1 movement every 10 minutes**

Movements per airport:

**960 ÷ 10 = 96 ≈ 100 movements/day**

Total medium-airport movements:

**70 × 100 = 7,000 movements/day**

Takeoffs:

**7,000 × 50% = 3,500 takeoffs/day**

---

### Step 3: Small Airports

Number of small airports:

**40**

Operating time per airport:

**10 hours × 60 minutes = 600 minutes/day**

Movement frequency:

**1 movement every 30 minutes**

Movements per airport:

**600 ÷ 30 = 20 movements/day**

Total small-airport movements:

**40 × 20 = 800 movements/day**

Takeoffs:

**800 × 50% = 400 takeoffs/day**

---

## 6. Final Calculation

Total takeoffs:

**Major = 3,000**

**Medium = 3,500**

**Small = 400**

Therefore:

**3,000 + 3,500 + 400 = 6,900 takeoffs/day**

## Final Estimate

> **Approximately 6,900 ≈ 7,000 commercial passenger flights take off across India in a normal day.**
