# 📊 Bina.az Real Estate Exploratory Data Analysis (EDA) Report

## 📌 1. Executive Summary
This report presents a comprehensive Exploratory Data Analysis (EDA) conducted on real estate listing data scraped from **Bina.az**. The main objective is to identify key pricing dynamics, structural correlations, geographical concentration, and categorical distribution patterns within the Baku real estate market. 

The findings provide strategic business insights for pricing models, platform monetization, and predictive feature engineering in subsequent Machine Learning steps.

---

## 🛠️ 2. Dataset Overview & Data Dictionary
The dataset contains active listings with structural features, numerical metrics, spatial coordinates, and engineered domain-specific features:

| Feature Name | Type | Description |
| :--- | :--- | :--- |
| `price` | Numerical | Total listing price in AZN |
| `unit_price` | Numerical | Price per square meter ($\text{AZN/m}^2$) |
| `Sahə` | Numerical | Total property area in square meters ($\text{m}^2$) |
| `Otaq sayı` | Numerical | Total room count |
| `views` | Numerical | Total number of listing views on the platform |
| `lat` / `lng` | Geographical | Latitude and Longitude coordinates |
| `Kateqoriya` | Categorical | Property type (*Yeni tikili*, *Köhnə tikili*, *Obyekt*, etc.) |
| `price_category` | Categorical (Engineered) | Price tier (*Low*, *Medium*, *High*, *Luxury*) |
| `area_per_room` | Numerical (Engineered) | Average spatial area per room ($\text{m}^2/\text{room}$) |

---

## 📊 3. Statistical Distribution & Visual Findings

### A. Price & Unit Price Distributions
- **Overall Price (`price`):** Exhibits a heavy right-skewed distribution. Applying a logarithmic transformation reveals a unimodal log-normal shape. The median property price stands at **220,000 AZN**, whereas the mean is pulled up to **304,818 AZN** due to high-value luxury and commercial outliers.
- **Unit Price (`unit_price`):** Unlike total price, price per square meter displays a highly symmetric distribution. The median (**2,230.0 AZN/m²**) and mean (**2,266.6 AZN/m²**) are almost identical, indicating a stable baseline unit value across standard residential inventory.

### B. Spatial & Engagement Distributions
- **Property Area (`Sahə`):** The living area peaks around standard architectural floor plans, with a median of **102.0 m²** and a mean of **164.6 m²**. Multi-modal spikes appear at standard intervals ($60\text{ m}^2$, $100\text{ m}^2$, $200\text{ m}^2$).
- **Listing Views (`views`):** Highly positively skewed platform engagement. The median view count is **252 views**, while the mean reaches **691 views** due to viral or featured listings exceeding 10,000 views.

### C. Market Structure & Categorical Breakdown
- **Property Types:** Market volume is heavily dominated by residential listings:
  - **Yeni tikili** (New Buildings): **59.0%** ($57,838$ listings)
  - **Köhnə tikili** (Old Buildings): **18.4%** ($18,090$ listings)
  - **Həyət evi/Bağ evi** (Houses/Villas): **15.9%** ($15,551$ listings)
  - **Commercial & Others** (*Obyekt*, *Torpaq*, *Ofis*, *Qaraj*): $< 7.0\%$ combined.
- **Price Tiers:** 
  - **Medium** ($60.5\%$, $59,318$ listings)
  - **High** ($27.7\%$, $27,134$ listings)
  - **Low** ($9.2\%$, $9,007$ listings)
  - **Luxury** ($2.7\%$, $2,636$ listings)

---

## 🔑 4. Key Business Insights from EDA

### 1. High Skewness in Market Valuation vs. Standardized Unit Pricing
Overall property listing prices exhibit strong right-skewness, driven by high-value luxury listings and commercial properties that inflate the mean. However, the price per square meter (`unit_price`) follows a remarkably stable and symmetric log-normal distribution across the market.

**Data evidence:**
- Total property price (`price`) shows a median of **220,000 AZN** compared to a significantly higher mean of **304,818 AZN**.
- Unit price (`unit_price`) maintains almost identical central values with a median of **2,230.0 AZN/m²** and a mean of **2,266.6 AZN/m²**, reflecting a consistent valuation baseline.

---

### 2. Physical Spatial Metrics as Primary Linear Price Drivers
Total property area (`Sahə`) and area per room (`area_per_room`) serve as the strongest linear determinants of total property price. Additionally, a significant level of spatial multicollinearity exists between these features.

**Data evidence:**
- `area_per_room` ($r = 0.62$) and total area (`Sahə`) ($r = 0.54$) display the highest positive correlations with overall property price.
- `Sahə` and `area_per_room` are heavily correlated with each other ($r = 0.75$), requiring cautious feature handling during modeling to prevent inflated variance and coefficient distortion.

---

### 3. Residential Volume Dominance and Tiered Market Segmentation
The supply side of the market is heavily dominated by residential properties, particularly new constructions (*Yeni tikili*). Price tier segmentation shows that the vast majority of active inventory sits squarely within the medium price bracket.

**Data evidence:**
- **Yeni tikili** (New Buildings) accounts for **59.0%** ($57,838$ listings), followed by **Köhnə tikili** (Old Buildings) at **18.4%** ($18,090$ listings) and **Həyət evi/Bağ evi** (Houses/Villas) at **15.9%** ($15,551$ listings).
- By `price_category`, **60.5%** ($59,318$) of properties belong to the **Medium** tier, **27.7%** ($27,134$) to the **High** tier, while the **Luxury** tier comprises only **2.7%** ($2,636$) of overall inventory.

---

### 4. High-Value Commercial Outliers and Industrial/Prime Location Premiums
Despite representing a small percentage of total listing volume, commercial properties (*Obyekt* and *Ofis*) command the highest average market prices. Similarly, industrial hubs and prime coastal/central zones heavily skew regional valuation averages.

**Data evidence:**
- **Obyekt** ($3.7\%$ share) averages **1.03M AZN** and **Ofis** ($0.2\%$ share) averages **777K AZN**, vastly exceeding the **284K AZN** average for **Yeni tikili**.
- **Qaradağ r.** and **Ulduz m.** top geographic valuations at **1.34M AZN** due to large commercial and industrial tracts, followed by premium residential coastal and central zones such as **Bilgəh q.** (**689K AZN**) and **Ağ şəhər q.** (**658K AZN**).

---

### 5. Highly Engagement-Skewed Platform Traffic Patterns
Listing view counts (`views`) demonstrate an extreme positive skewness, where a small subset of viral or promoted listings heavily inflates the platform average engagement figures.

**Data evidence:**
- View count shows a median of **252 views** versus a far higher mean of **691 views**.
- `views` maintains weak positive correlations with total price ($r = 0.11$) and `area_per_room` ($r = 0.11$), indicating that while users browse larger/luxury listings slightly more often, view counts are primarily driven by listing duration on platform or promotional status.

---

## 🚀 5. Recommendations for Subsequent Modeling Steps
1. **Target Variable Transformation:** Apply $\log(1 + \text{price})$ transformation to stabilize variance and remove right-skewness effect during model training.
2. **Handling Multicollinearity:** Either drop or apply regularization (Lasso/Ridge) to `Sahə` and `area_per_room` ($r = 0.75$) to avoid collinearity bias.
3. **Spatial Feature Engineering:** Since linear coordinates (`lat`, `lng`) show low direct correlation with price ($r \approx 0.05$), non-linear spatial encoding (e.g., target encoding by rayon/suburb or spatial clustering like K-Means) should be implemented.
