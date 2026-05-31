# Seventh_Project_Devlab

# 🏡 Airbnb Pricing & Occupancy Analytics by Neighbourhood

> **Dataset:** [Seattle Airbnb Open Data](https://www.kaggle.com/datasets/airbnb/seattle)  
> **Level:** Advanced  
> **Tools:** Python · pandas · matplotlib · seaborn · Jupyter

---

## 📋 Project Overview

This project analyses Seattle Airbnb listing and calendar data to:

- Benchmark nightly prices across all neighbourhoods using median statistics
- Derive occupancy rates from calendar availability data
- Identify seasonal pricing patterns across 12 months
- Calculate per-neighbourhood **revenue potential** (the key metric for investors)
- Compare Instant Bookable vs non-Instant Bookable listing performance

---

## 📁 Repository Structure

```
airbnb_pricing_occupancy_analytics/
│
├── airbnb_pricing_occupancy_analytics.ipynb   ← Main analysis notebook
├── README.md                                  ← This file
│
├── listings.csv                               ← (download from Kaggle)
├── calendar.csv                               ← (download from Kaggle)
│
└── outputs/                                   ← Generated on notebook run
    ├── viz_neighbourhood_price.png
    ├── viz_seasonal_price.png
    ├── viz_roomtype_price.png
    ├── viz_occupancy_reviews.png
    ├── viz_instant_bookable.png
    ├── viz_revenue_potential.png
    └── viz_correlation_matrix.png
```

---

## ⚙️ Setup

### 1. Install dependencies

```bash
pip install pandas numpy matplotlib seaborn jupyter
```

### 2. Download the dataset

1. Go to [kaggle.com/datasets/airbnb/seattle](https://www.kaggle.com/datasets/airbnb/seattle)
2. Download and unzip; place **`listings.csv`** and **`calendar.csv`** in the same folder as the notebook.

### 3. Run the notebook

```bash
jupyter notebook airbnb_pricing_occupancy_analytics.ipynb
```

Run all cells top to bottom (`Kernel → Restart & Run All`).

---

## 🔬 Methodology

### Data Loading & Cleaning

| File | Shape | Key Cleaning Steps |
|------|-------|--------------------|
| `listings.csv` | 3,818 rows × 92 cols | Strip `$` and `,` from `price`; cast to `float`; drop rows with missing price or neighbourhood; remove extreme outliers (price > $1,500) |
| `calendar.csv` | 1,393,570 rows × 5 cols | Parse `date` to `datetime`; map `available` (`t`/`f`) to binary (1/0); fill `NaN` prices with per-listing calendar median |

### Occupancy Rate Calculation

```
occupancy_rate = booked_days / total_calendar_days
booked_days    = days where available == 0 ("f")
```

Computed per `listing_id` over the full calendar window (~365 days), then merged back to `listings` on `id`.

### Neighbourhood Benchmarking

Aggregated by `neighbourhood_cleansed`:
- **Median price** — robust to outliers
- **Average occupancy rate**
- **Listing count**
- **Revenue potential** = `avg_occupancy × median_price × 365`

### Seasonal Analysis

Monthly median price extracted from `calendar.csv` for **booked nights only**, spanning January – December. This reflects actual transaction prices rather than listed prices.

### Statistical Correlations

- `number_of_reviews` vs `occupancy_rate` → Pearson r
- `minimum_nights` vs `price` → Pearson r
- Full correlation matrix across 5 key numeric fields

---

## 📊 Visualisations

| File | Description |
|------|-------------|
| `viz_neighbourhood_price.png` | Horizontal bar chart — Top 5 & Bottom 5 neighbourhoods by median nightly price |
| `viz_seasonal_price.png` | Line chart — monthly median price showing summer peak |
| `viz_roomtype_price.png` | Boxplot — price distribution by room type (Entire home / Private room / Shared room) |
| `viz_occupancy_reviews.png` | Scatter plot with trend line — occupancy rate vs number of reviews |
| `viz_instant_bookable.png` | Side-by-side bar — Instant Book vs non-Instant: price & occupancy |
| `viz_revenue_potential.png` | Horizontal bar — Top 10 neighbourhoods by annual revenue potential |
| `viz_correlation_matrix.png` | Lower-triangle heatmap — key metric correlations |

---

## 💡 5 Key Insights for Hosts & Investors

### 1. The Pricing Gap is Enormous
Southeast Magnolia commands ~$175/night (median) while Rainier Beach sits at ~$65/night — a **≥2.5× spread**. Premium location is the single biggest pricing lever available to a host.

### 2. Price ≠ Revenue Potential
The most expensive neighbourhood is not the best investment. Capitol Hill, with a 74% occupancy rate, outperforms pricier but lower-occupancy areas on **annual revenue potential** (occupancy × price × 365). Always evaluate *yield*, not just rack rate.

### 3. Capture the Summer Premium (+28%)
Median booked prices in June–August are approximately **28% higher** than December–February. Hosts who leave pricing on "flat" lose significant seasonal revenue. Tools like Airbnb Smart Pricing, PriceLabs, or Wheelhouse pay for themselves quickly.

### 4. Instant Book: Accept Lower Price for Higher Volume
Instant Bookable listings are priced ~8% lower on average but achieve ~12% higher occupancy. For **new hosts** who need to build review count rapidly, enabling Instant Book is the fastest path to social proof — trade margin for momentum, then reprice once established.

### 5. Reviews Are a Compounding Asset (r = 0.41)
The moderate positive correlation between review count and occupancy is the strongest actionable signal in the dataset. Every review raises your listing's ranking and conversion rate. Implement a post-checkout message sequence and respond to all reviews within 24 hours.

---

## 🔑 Quick Reference: Key Numbers

| Metric | Value |
|--------|-------|
| Listings analysed | ~3,800 |
| Calendar rows | ~1.4 million |
| Neighbourhoods | 80+ |
| Overall median price | ~$100/night |
| Overall avg occupancy | ~55% |
| Most expensive neighbourhood | Southeast Magnolia (~$175/night) |
| Least expensive neighbourhood | Rainier Beach (~$65/night) |
| Best revenue potential | Capitol Hill |
| Summer vs Winter premium | +28% |
| Instant Book occupancy uplift | +12% |
| Reviews → Occupancy correlation | r = 0.41 |
| Minimum nights → Price correlation | r = 0.14 |

---

## 📝 Notes on Reproducibility

- All random seeds are implicit (no ML models used — pure aggregation/statistics).
- Price outliers above $1,500/night are dropped to prevent skew in median calculations; this removes fewer than 0.5% of listings.
- Occupancy rates for listings with fewer than 30 calendar days of data should be interpreted with caution.
- The dataset reflects a specific snapshot of Seattle Airbnb supply; conclusions about trends require multi-year data.

---

## 📜 License

Dataset © Airbnb / Kaggle — used for educational purposes only.  
Analysis code: MIT License.
