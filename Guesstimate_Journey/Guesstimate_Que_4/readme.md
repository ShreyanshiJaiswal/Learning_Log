# Guesstimate 4: Number of Aeroplanes in Air Right Now

## Clarifying Questions

1. Are we estimating the number of aeroplanes in India or globally?
2. Are we estimating during a peak time or a normal time?
3. Are we considering only commercial aeroplanes or also cargo, fighter, private, etc.?
4. Are we considering domestic flights or both domestic and international flights?

### Final Question

**Estimate the number of domestic aeroplanes flying over India for commercial purposes at peak time.**

---

## Solution

### Key Logic

The number of aeroplanes in the air at any point in time depends on:

**Number of aeroplanes in air = Number of flights taking off per hour × Average flight duration**

So, we first estimate the number of flights taking off from Indian airports per hour and then multiply it by the average duration for which each flight remains in the air.

---

## Step 1: Divide Operational Airports into Categories

Assume there are **90 operational airports** in India.

We divide them into three categories:

| Airport Type | Number of Airports | Runways per Airport | Average Takeoff Interval |
|---|---:|---:|---:|
| Mega | 10 | 3 | 5 minutes |
| Mid | 30 | 2 | 20 minutes |
| Small | 50 | 1 | 30 minutes |

---

## Step 2: Calculate Flights Taking Off per Hour

### Mega Airports

Assumptions:
- 10 mega airports
- 3 runways per airport
- One flight takes off every 5 minutes per runway

Flights per runway per hour:

**60 ÷ 5 = 12 flights**

Therefore:

**10 × 12 × 3 = 360 flights/hour**

So, mega airports contribute approximately **360 flights per hour**.

---

### Mid Airports

Assumptions:
- 30 mid-sized airports
- 2 runways per airport
- One flight takes off every 20 minutes per runway

Flights per runway per hour:

**60 ÷ 20 = 3 flights**

Therefore:

**30 × 3 × 2 = 180 flights/hour**

So, mid-sized airports contribute approximately **180 flights per hour**.

---

### Small Airports

Assumptions:
- 50 small airports
- 1 runway per airport
- One flight takes off every 30 minutes

Flights per runway per hour:

**60 ÷ 30 = 2 flights**

Therefore:

**50 × 2 × 1 = 100 flights/hour**

So, small airports contribute approximately **100 flights per hour**.

---

## Step 3: Calculate Total Flights Taking Off per Hour

**Total flights per hour**

= Mega + Mid + Small

= 360 + 180 + 100

= **640 flights/hour**

Therefore, we estimate approximately **640 domestic commercial flights take off per hour** during peak time.

---

## Step 4: Calculate Number of Aeroplanes in the Air

Assume the average domestic flight duration is **2 hours**.

This means that every flight taking off remains in the air for approximately 2 hours.

Therefore:

**Aeroplanes in air**

= Flights taking off per hour × Average flight duration

= 640 × 2

= **1,280 aeroplanes**

---

## Final Answer

### **Estimated number of domestic commercial aeroplanes flying over India at peak time ≈ 1,280**

### Summary

**90 operational airports**
↓
**10 Mega + 30 Mid + 50 Small airports**
↓
**360 + 180 + 100 flights/hour**
↓
**640 flights taking off per hour**
↓
**Average flight duration = 2 hours**
↓
**640 × 2**
↓
### **≈ 1,280 aeroplanes in the air**
