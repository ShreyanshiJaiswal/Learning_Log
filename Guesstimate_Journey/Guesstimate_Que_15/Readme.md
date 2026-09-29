# Guesstimate 15: Estimate the Number of Street Lights in India

## Clarifying Questions

- Are we considering a specific city, district, or state?
- Are we considering urban areas, rural areas, or both?
- Are we considering only major cities or the entire country?
- Should we include main roads as well as outskirts and local roads?
- Are we considering all types of street lights such as LED, fluorescent, and solar?
- Are we estimating the currently installed street lights or annual replacement demand?

## Final Question

Estimate the total number of street lights currently installed across **India**, including **urban areas, rural areas, main roads, outskirts, and all types of street lights**.

## Solution

### Key Logic

**Total Road Length → Urban/Rural Split → Main/Outskirts Split → % Roads with Street Lighting → Lights per km → Total Street Lights**

### Step 1: Estimate Total Road Length

Assume India's total road network is approximately:

**6.37 million km**

### Step 2: Split Roads into Urban and Rural

India is still predominantly rural, but rapid urbanization and expansion of cities mean urban roads represent a significant share of the road network.

Assumption:

- Urban roads = **35%**
- Rural roads = **65%**

Therefore:

**Urban roads**

= 6.37M × 35%  
= **2.23 million km**

**Rural roads**

= 6.37M × 65%  
= **4.14 million km**

### Step 3: Split Urban and Rural Roads into Main Roads and Outskirts/Local Roads

The road distribution is different for urban and rural areas.

#### Urban

Urban areas have a relatively balanced distribution between major roads and outskirts/local roads.

- Main roads = **50%**
- Outskirts/local roads = **50%**

#### Rural

Rural areas have a much larger share of local and lower-volume roads, with only a smaller portion being major roads.

- Main roads = **20%**
- Outskirts/local roads = **80%**

### Step 4: Estimate Percentage of Roads with Street Lighting

Not every road is continuously street-lit.

Assumptions:

| Area | % of Roads with Street Lighting | Lights per km |
|---|---:|---:|
| Urban – Main Roads | 50% | 20 |
| Urban – Outskirts | 30% | 10 |
| Rural – Main Roads | 20% | 10 |
| Rural – Outskirts | 10% | 5 |

### Step 5: Calculate Urban Main-Road Street Lights

Urban roads = **2.23M km**

Main roads = 50%

Street-lit roads = 50%

Lights per km = 20

**2.23M × 50% × 50% × 20**

= **11.15M street lights**

### Step 6: Calculate Urban Outskirts Street Lights

Urban roads = **2.23M km**

Outskirts/local roads = 50%

Street-lit roads = 30%

Lights per km = 10

**2.23M × 50% × 30% × 10**

= **3.35M street lights**

### Step 7: Calculate Rural Main-Road Street Lights

Rural roads = **4.14M km**

Main roads = 20%

Street-lit roads = 20%

Lights per km = 10

**4.14M × 20% × 20% × 10**

= **1.66M street lights**

### Step 8: Calculate Rural Outskirts/Local-Road Street Lights

Rural roads = **4.14M km**

Outskirts/local roads = 80%

Street-lit roads = 10%

Lights per km = 5

**4.14M × 80% × 10% × 5**

= **1.66M street lights**

### Step 9: Calculate Total Street Lights

Total street lights:

= Urban Main + Urban Outskirts + Rural Main + Rural Outskirts

= 11.15M + 3.35M + 1.66M + 1.66M

= **17.82M**

### Final Estimate

**≈ 18 million street lights in India**

> **Final Answer: Approximately 18 million street lights**
