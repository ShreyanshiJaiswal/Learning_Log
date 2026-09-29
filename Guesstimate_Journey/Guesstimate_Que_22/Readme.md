# Guesstimate 22: Estimate Liters of Petrol Sold at a Petrol Pump

## Question

Estimate the **liters of petrol sold at a petrol pump per day**.

---

## Clarifying Questions

1. Are we estimating petrol sold daily, weekly, monthly, or yearly?
2. Are we considering peak and non-peak hours?
3. Are we considering all vehicle types?
4. What is the location of the petrol pump?
5. What is the size of the petrol pump - small, medium, or high-volume?
6. Is there a nearby petrol pump that could divert customers?

---

## Final Question

Estimate the **daily liters of petrol sold throughout the day at a medium-sized petrol pump in a Tier-1 city**, considering all relevant vehicle types and assuming **no nearby competing petrol pump**.

---

# Solution

## Key Logic

The estimation can be broken down into:

**Total vehicles served per day**
→ **Vehicle segmentation**
→ **Average petrol purchased per vehicle per visit**
→ **Petrol sold by each vehicle category**
→ **Total petrol sold per day**

---

## Step 1: Estimate Total Vehicles Served Per Day

Assume the medium-sized petrol pump operates for approximately **18 hours per day**.

A medium-sized petrol pump is assumed to have approximately **4-6 dispensers**.

Assume each vehicle takes approximately **2-5 minutes** for refueling.

### Theoretical Capacity

For one dispenser:

- At 5 minutes per vehicle:

  `60 / 5 = 12 vehicles/hour`

- At 2 minutes per vehicle:

  `60 / 2 = 30 vehicles/hour`

Therefore, for 4-6 dispensers:

| Number of Dispensers | At 5 min/vehicle | At 2 min/vehicle |
|---:|---:|---:|
| 4 | 48 vehicles/hour | 120 vehicles/hour |
| 5 | 60 vehicles/hour | 150 vehicles/hour |
| 6 | 72 vehicles/hour | 180 vehicles/hour |

This is the **theoretical maximum capacity**. Actual throughput will be lower because of vehicle positioning, payment time, idle periods, and variations in customer demand.

Therefore, we assume the following practical throughput:

| Period | Hours | Vehicles/Hour | Vehicles Served |
|---|---:|---:|---:|
| Peak | 6 | 100 | 600 |
| Normal | 8 | 70 | 560 |
| Non-peak | 4 | 30 | 120 |
| **Total** | **18** | - | **1,280** |

### Calculation

`Peak vehicles = 6 × 100 = 600`

`Normal vehicles = 8 × 70 = 560`

`Non-peak vehicles = 4 × 30 = 120`

`Total vehicles = 600 + 560 + 120 = 1,280`

Round this to:

### **~1,300 vehicles per day**

---

## Step 2: Segment Vehicles

For a Tier-1 city, assume the vehicles visiting the petrol pump are distributed as follows:

| Vehicle Type | Assumed Share | Vehicles/Day |
|---|---:|---:|
| Two-wheelers | 50% | 650 |
| Four-wheelers | 40% | 520 |
| Petrol-powered commercial/other vehicles | 10% | 130 |
| **Total** | **100%** | **1,300** |

### Assumption: Two-Wheelers

Two-wheelers are assumed to form the largest segment because they are widely used for urban commuting and are convenient in congested Tier-1 city traffic.

### Assumption: Four-Wheelers

Four-wheelers form another major segment of urban traffic and generally require more petrol per visit than two-wheelers.

### Assumption: Commercial/Other Vehicles

Many buses and heavy trucks typically use diesel rather than petrol. Therefore, this category represents **petrol-powered commercial and other vehicles** rather than assuming all buses and trucks use petrol.

---

## Step 3: Estimate Petrol Purchased Per Visit

Tank capacity does not mean that the vehicle fills its entire tank during every visit.

Therefore, we estimate the **average quantity of petrol purchased per visit**.

| Vehicle Type | Typical Tank Capacity | Refueling Pattern | Assumed Petrol per Visit |
|---|---:|---|---:|
| Two-wheelers | 8-15 L | Every 3-4 days | 3 L |
| Four-wheelers | 40-60 L | Approximately weekly | 10 L |
| Petrol-powered commercial/other vehicles | Higher capacity | More frequent refueling | 30 L |

The assumed petrol quantity is the **amount purchased per visit**, not the total tank capacity.

---

## Step 4: Calculate Petrol Sold by Vehicle Type

### Two-Wheelers

`650 vehicles × 3 L = 1,950 L/day`

### Four-Wheelers

`520 vehicles × 10 L = 5,200 L/day`

### Petrol-Powered Commercial/Other Vehicles

`130 vehicles × 30 L = 3,900 L/day`

---

## Step 5: Calculate Total Petrol Sold

| Vehicle Type | Vehicles/Day | Petrol/Vehicle/Visit | Petrol Sold/Day |
|---|---:|---:|---:|
| Two-wheelers | 650 | 3 L | 1,950 L |
| Four-wheelers | 520 | 10 L | 5,200 L |
| Petrol-powered commercial/other vehicles | 130 | 30 L | 3,900 L |
| **Total** | **1,300** | - | **11,050 L** |

### Calculation

`Total petrol sold = 1,950 + 5,200 + 3,900`

`Total petrol sold = 11,050 L/day`

---

# Final Estimate

## **~11,000 Liters of Petrol Sold Per Day**

Therefore, our estimate for a **medium-sized petrol pump in a Tier-1 city** is approximately:

### **11,050 liters of petrol per day**
