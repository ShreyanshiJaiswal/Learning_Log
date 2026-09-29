# Guesstimate 6 — Automotive Battery Market in India

## Clarifying Questions

1. Are we estimating the market for a particular month or the entire year?
2. Are we estimating the market for all of India or a particular city/tier?
3. Are we considering batteries used in EVs as well as traditional vehicles?
4. Are we considering both 2-wheelers and 4-wheelers?

## Final Question

What is the estimated **automotive battery market size in India for one full year**, considering both EV and traditional vehicles across 2-wheelers and 4-wheelers?

## Solution

### Key Logic

Population → Urban/Rural → Rich/Middle/Poor → Vehicle Ownership → Total Vehicles → 2-Wheeler/4-Wheeler → EV/Traditional → Battery Cost → Replacement Frequency → **Annual Battery Market**

### Step 1: Estimate Total Vehicles

India's population = **1.4 billion**

Assumptions:

- Urban population = 40%
- Rural population = 60%

#### Urban Population

Urban population:

**1.4B × 40% = 560M**

Assume:

- Rich = 30%
- Middle = 50%
- Poor = 20%

Vehicle ownership assumptions:

- Rich = 3 vehicles per person
- Middle = 2 vehicles per person
- Poor = 1 vehicle per person

| Segment | Population | Vehicles per Person | Total Vehicles |
|---|---:|---:|---:|
| Rich | 560M × 30% = 168M | 3 | 504M |
| Middle | 560M × 50% = 280M | 2 | 560M |
| Poor | 560M × 20% = 112M | 1 | 112M |
| **Total** | **560M** | | **1,176M** |

**Urban vehicles = 1.176B**

#### Rural Population

Rural population:

**1.4B × 60% = 840M**

Assume:

- Rich = 10%
- Middle = 50%
- Poor = 40%

Vehicle ownership assumptions:

- Rich = 1 vehicle per person
- Middle = 0 vehicles
- Poor = 0 vehicles

| Segment | Population | Vehicles per Person | Total Vehicles |
|---|---:|---:|---:|
| Rich | 840M × 10% = 84M | 1 | 84M |
| Middle | 840M × 50% = 420M | 0 | 0 |
| Poor | 840M × 40% = 336M | 0 | 0 |
| **Total** | **840M** | | **84M** |

**Rural vehicles = 84M**

### Step 2: Calculate Total Vehicles

**Urban vehicles + Rural vehicles**

= 1,176M + 84M

= **1,260M**

**Total vehicles = 1.26 billion**

### Step 3: Split Vehicles into 2-Wheelers and 4-Wheelers

Assume:

- 2-wheelers = 60%
- 4-wheelers = 40%

| Vehicle Type | Share | Number of Vehicles |
|---|---:|---:|
| 2-Wheelers | 60% | 1,260M × 60% = **756M** |
| 4-Wheelers | 40% | 1,260M × 40% = **504M** |
| **Total** | **100%** | **1,260M** |

### Step 4: Split 2-Wheelers into EV and Traditional

Assume:

- 2W EV = 30%
- 2W Traditional = 70%

| 2-Wheeler Type | Share | Vehicles |
|---|---:|---:|
| EV | 30% | 756M × 30% = **226.8M** |
| Traditional | 70% | 756M × 70% = **529.2M** |
| **Total** | **100%** | **756M** |

### Step 5: Split 4-Wheelers into EV and Traditional

Assume:

- 4W EV = 20%
- 4W Traditional = 80%

| 4-Wheeler Type | Share | Vehicles |
|---|---:|---:|
| EV | 20% | 504M × 20% = **100.8M** |
| Traditional | 80% | 504M × 80% = **403.2M** |
| **Total** | **100%** | **504M** |

### Step 6: Battery Cost and Replacement Assumptions

We assume the following average battery costs and replacement frequencies:

| Vehicle Type | Battery Type | Average Battery Cost | Replacement Frequency |
|---|---|---:|---:|
| 2-Wheeler | EV | ₹10,000 | Every 5 years |
| 2-Wheeler | Traditional | ₹2,000 | Every 2 years |
| 4-Wheeler | EV | ₹2,00,000 | Every 5 years |
| 4-Wheeler | Traditional | ₹20,000 | Every 2 years |

### Step 7: Estimate Annual Battery Market

Formula:

**Annual Battery Market = Number of Vehicles × Battery Cost ÷ Replacement Frequency**

| Vehicle Type | Battery Type | Vehicles | Battery Cost | Replacement Frequency | Annual Battery Market |
|---|---|---:|---:|---:|---:|
| 2-Wheeler | EV | 226.8M | ₹10,000 | 5 years | **₹45,360 Cr** |
| 2-Wheeler | Traditional | 529.2M | ₹2,000 | 2 years | **₹52,920 Cr** |
| **2-Wheeler Total** | | **756M** | | | **₹98,280 Cr** |
| 4-Wheeler | EV | 100.8M | ₹2,00,000 | 5 years | **₹4,03,200 Cr** |
| 4-Wheeler | Traditional | 403.2M | ₹20,000 | 2 years | **₹4,03,200 Cr** |
| **4-Wheeler Total** | | **504M** | | | **₹8,06,400 Cr** |
| **Total Automotive Battery Market** | | **1.26B** | | | **₹9,04,680 Cr** |

## Final Estimate

**Annual Automotive Battery Market**

= 2-Wheeler Market + 4-Wheeler Market

= ₹98,280 Cr + ₹8,06,400 Cr

= **₹9,04,680 Cr**

Therefore:

**Estimated automotive battery market in India for one full year ≈ ₹9.05 lakh crore**
