# madewell-inventory-analysis
# Smart Inventory Management System: Madewell Denim

## Overview

This project builds a data-driven inventory management system for a fictional 
Madewell denim product line, replicating the core workflow of a retail inventory 
planning analyst. Using synthetically generated but realistic data, the system 
moves from raw demand signals to actionable reorder recommendations.

**Tools:** Python, Jupyter Notebook, pandas, scikit-learn, XGBoost, statsmodels, Plotly

---

## Business Problem

Retail inventory planning involves two competing risks:
- **Stockout risk** — running out of stock and losing sales
- **Markdown risk** — overstocking and being forced to discount

This project builds a system that forecasts demand, predicts both risks, and 
recommends optimal reorder quantities to keep inventory in balance.

---

## Project Structure

| Step | Analysis | Method |
|------|----------|--------|
| 0 | Data Generation | Synthetic SKU data with realistic seasonality |
| 1 | Demand Forecasting | Holt-Winters Exponential Smoothing (ETS) |
| 2 | Markdown Risk Prediction | XGBoost Classifier |
| 3 | Stockout Risk Prediction | XGBoost Classifier |
| 4 | Reorder Optimization | EOQ + Safety Stock + Reorder Point |
| 5 | Executive Dashboard | Interactive Plotly dashboard |

---

## Dataset

The dataset was synthetically generated to simulate 2 years of weekly sales 
data across 20 Madewell denim SKUs. SKUs span three categories (Core, Trend, 
Relaxed), four silhouettes (Straight, Skinny, Wide-Leg, Flare, Boyfriend), 
and four washes (Dark, Medium, Light, Black).

Demand curves embed realistic seasonal patterns:
- **Week 14** (early April) — spring refresh
- **Week 36** (early September) — back-to-school
- **Week 48** (late November) — holiday shopping

Trend SKUs (Wide-Leg, Flare) show +8% demand growth year-over-year. 
Skinny SKUs show a slight decline, reflecting real market trends.

---

## Key Findings

**Demand Forecasting**
- Spring (March–April) is the highest revenue period, driven by seasonal demand 
  peaking around week 14
- The Holt-Winters model captures three distinct seasonal spikes per year with 
  strong fit across all Core SKUs

**Markdown & Stockout Risk**
- `Weeks_of_Supply` is the strongest predictor of markdown risk — when a SKU 
  has more than 6 weeks of stock relative to current demand, markdown pressure builds
- `Ending_Stock` is the strongest predictor of stockout risk, with stockout 
  events concentrated during peak demand weeks
- Both models achieved ROC-AUC > 0.99, reflecting that the target variables 
  were derived from features present in the training data. In production, 
  these models would predict risk N weeks ahead using only features available 
  at that point in time

**Reorder Optimization**
- All 20 SKUs are flagged as High Stockout Risk under the baseline reorder 
  system, indicating systematic under-stocking
- EOQ analysis recommends order quantities of 87–139 units depending on SKU 
  price and velocity
- Core SKUs require significantly more safety stock (avg 42 units) than Trend 
  (30) or Relaxed (25) because higher absolute demand volume creates larger 
  absolute demand swings even at similar volatility percentages

---

## How to Run

1. Clone the repo
2. Install dependencies:
pip install pandas numpy matplotlib seaborn scikit-learn xgboost statsmodels plotly
3. Open `madewell_inventory.ipynb` in Jupyter Notebook and run all cells

---

## Limitations & Next Steps

- **Synthetic data** — results reflect the patterns we built into the data 
  generator. A production version would use real POS and inventory data
- **Single-location model** — does not account for multi-store inventory 
  pooling or inter-store transfers
- **Static lead time** — assumes a fixed 4-week replenishment lead time; 
  real systems would model lead time variability
- **Next steps:** integrate real Madewell sell-through data via web scraping, 
  extend to a multi-location model, add a markdown timing recommender

---

*Part of a retail data science portfolio. See also: 
[Fishwife Tinned Seafood Brand Analysis](https://github.com/sadiegberg/Fishwife-analysis) | 
[Lime Scooter Demand Analysis](https://github.com/sadiegberg/Lime-Scooter-Analysis)*
