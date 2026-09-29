
# Guesstimate 25: Cars Crossing Delhi-Gurgaon Toll

## Clarifying Questions

- Are we estimating traffic for a **day, month, or year**?
- Should we consider **both directions**?
- Should we include **all types of cars**, such as private cars, cabs, rented cars, company and government vehicles?
- Should we consider the **entire 24 hours**, including peak and non-peak periods?
- How many **lanes** are operational in total and in each direction?
- Should we estimate a **weekday or weekend**?

## Final Question

Estimate the number of **cars crossing the Delhi-Gurgaon toll in one day**, considering both directions, all types of cars, and the entire 24-hour period.

## Solution

### Key Logic

- Assume approximately **16 toll lanes**, with 8 lanes in each direction.
- A lane can theoretically process around **700–800 cars/hour**.
- Use **800 cars/hour/lane** as the approximate maximum throughput.
- Actual traffic will be lower because lanes are not utilized at 100% throughout the day.
- Morning traffic is heavier towards Gurgaon, while evening traffic is heavier towards Delhi.
- Apply different utilization rates for peak and non-peak periods.

### Step 1: Maximum Capacity of One Lane

Assume one car takes approximately **5 seconds** to pass through the toll.

- Cars per minute = 60 ÷ 5 = **12 cars**
- Cars per hour = 12 × 60 = **720 cars**

Round this to approximately **800 cars/hour/lane** to account for variations in processing time.

### Step 2: Maximum Capacity of All Lanes

- Total lanes = **16**
- Capacity per lane = **800 cars/hour**

**16 × 800 = 12,800 cars/hour**

This represents the maximum theoretical capacity, not actual traffic.

### Step 3: Peak-Hour Traffic

Assume peak hours are:

- Morning: **7 AM – 11 AM**
- Evening: **5 PM – 9 PM**

Total peak period:

**8 hours**

Assume average lane utilization during peak hours is approximately **70%** because traffic is high but lanes may still not operate at full capacity continuously.

Peak traffic:

**12,800 × 70% × 8 = 71,680 cars**

### Step 4: Non-Peak Traffic

Remaining hours:

**24 − 8 = 16 hours**

Assume average lane utilization during non-peak hours is approximately **40%** due to lower traffic volume.

Non-peak traffic:

**12,800 × 40% × 16 = 81,920 cars**

### Step 5: Total Daily Traffic

Total cars:

**71,680 + 81,920 = 1,53,600 cars/day**

Round to approximately:

**1.5 lakh cars/day**

## Final Estimate

**Approximately 1.5 lakh cars cross the Delhi-Gurgaon toll in a day.**

## Factors Affecting the Estimate

- Weekday traffic is likely to be higher because of office commuters.
- Weekend traffic may be **10–15% lower**.
- Festivals, holidays, events, and favourable weather can increase traffic.
- Heavy congestion or road restrictions can reduce throughput.
- Actual lane utilization may vary significantly throughout the day.
- Airport, intercity, commercial, and daily commute traffic contribute to the overall volume.
