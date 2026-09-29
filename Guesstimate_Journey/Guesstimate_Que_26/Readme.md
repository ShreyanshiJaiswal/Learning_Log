# Guesstimate 26 — Revenue of a Petrol Pump

## Clarifying Questions

- Is the estimate for a day, month, or year?
- What type of location — urban, highway, or rural?
- What size of petrol pump — small, medium, or large?
- Does the pump sell both petrol and diesel?
- Are there nearby competing petrol pumps?
- Does it have additional facilities such as a convenience store, car wash, air filling, or lubricants?
- Should price sensitivity be considered?

## Final Question

Estimate the daily revenue of a medium-sized petrol pump in a Tier-1 urban city, with no nearby competing pump, selling both petrol and diesel and having a few additional facilities.

## Solution

### 1. Maximum Vehicles Served Per Day

Assume a medium-sized petrol pump has:

- 4 petrol dispensers
- 2 diesel dispensers
- Average refuelling time = 3 minutes

Therefore:

**Vehicles served per dispenser per hour = 60 ÷ 3 = 20**

**Maximum capacity = 6 dispensers × 20 = 120 vehicles/hour**

Assume the pump operates for **18 hours/day**:

**Maximum daily capacity = 120 × 18 = 2,160 vehicles/day**

### 2. Actual Vehicles Served Per Day

Maximum capacity represents the theoretical throughput, so assume:

- Peak hours: 12 hours × 70% utilization
- Non-peak hours: 6 hours × 40% utilization

Peak:

**120 × 70% × 12 = 1,008 vehicles**

Non-peak:

**120 × 40% × 6 = 288 vehicles**

**Total vehicles served = 1,296 vehicles/day**

### 3. Vehicle Type Split

| Vehicle Type | Share | Vehicles/Day |
|---|---:|---:|
| Two-wheelers | 50% | 648 |
| Four-wheelers | 40% | 518 |
| Others | 10% | 130 |
| **Total** | **100%** | **1,296** |

The vehicle mix is carried forward from the assumptions used in the previous petrol-volume guesstimate.

### 4. Petrol vs. Diesel Split

- Two-wheelers: **100% petrol**
- Four-wheelers: **60% petrol, 40% diesel**
- Others: **50% petrol, 50% diesel**

| Vehicle Type | Petrol | Diesel |
|---|---:|---:|
| Two-wheelers | 648 | 0 |
| Four-wheelers | 311 | 207 |
| Others | 65 | 65 |
| **Total** | **1,024** | **272** |

### 5. Average Fuel Purchased Per Vehicle

| Vehicle/Fuel Type | Vehicles | Avg. Fuel/Vehicle | Total Fuel |
|---|---:|---:|---:|
| 2W Petrol | 648 | 3 L | 1,944 L |
| 4W Petrol | 311 | 8 L | 2,488 L |
| Other Petrol | 65 | 15 L | 975 L |
| 4W Diesel | 207 | 12 L | 2,484 L |
| Other Diesel | 65 | 20 L | 1,300 L |

**Total petrol = 5,407 L/day**

**Total diesel = 3,784 L/day**

As a cross-check, the previous Guesstimate 25 estimated approximately **12,750 L/day of petrol** for a similar petrol-pump scenario. The lower petrol volume here is consistent with applying utilization to the pump's theoretical capacity rather than assuming full capacity throughout operating hours.

### 6. Fuel Revenue

Assume:

- Petrol price = **₹100/L**
- Diesel price = **₹90/L**

Petrol revenue:

**5,407 × ₹100 = ₹5,40,700**

Diesel revenue:

**3,784 × ₹90 = ₹3,40,560**

**Total fuel revenue = ₹8,81,260/day**

### 7. Revenue From Additional Services

Assume convenience store, lubricants, car wash, air filling and other services contribute approximately **10% of fuel revenue**.

**Additional revenue = ₹8,81,260 × 10% = ₹88,126**

## Final Estimate

**Total daily revenue = ₹8,81,260 + ₹88,126**

# **≈ ₹9.7 lakh/day**
