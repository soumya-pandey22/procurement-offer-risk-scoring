# 📦 Procurement Offer Risk Scoring

## 📋 Overview
This project scores and segments procurement offers/bids based on delivery, 
quality, and reliability metrics, using an RFM-style quantile scoring approach. 
Each offer is classified as **🟢 Preferred**, **🟡 Acceptable**, or **🔴 High Risk** 
based on a composite performance score.

## ⚠️ Important note on data structure
Each row in this dataset represents a **single procurement offer/bid**, not a 
repeat, tracked supplier. There is no supplier ID or name in the source data — 
so this analysis scores individual offers rather than aggregating performance 
history per named vendor. Segment labels ("Preferred," "High Risk") describe 
the offer, not a long-term vendor relationship.

## 🗂️ Dataset
- **Source:** [add where you got Supply_chain_dataset.csv]
- **Rows:** 3,089 offers, 22 columns
- **Key columns used:**
  - `on_time_delivery_rate`, `defect_rate`, `return_rate` — performance metrics
  - `price_per_unit`, `quality_score`, `supplier_reliability_score` — additional signals
  - `delivery_mode` — contextual/categorical field

## ⚙️ Methodology
1. **🧹 Cleaning:** checked for missing values (none found) and duplicates (none found)
2. **🎯 Scoring:** each performance metric converted to a 1–5 score using quantile 
   binning (`pd.qcut`), with "bad" metrics (defect rate, return rate) reverse-scored
3. **➕ Composite score:** summed individual scores into one `performance_score` 
   (range 5–25, mean ≈ 15.5)
4. **🏷️ Segmentation:** top 25% of scores → Preferred, bottom 25% → High Risk, 
   middle 50% → Acceptable
5. **📊 Visualization:** bar charts (segment averages, segment counts), scatter plot 
   (on-time delivery vs defect rate), correlation heatmap

## 🔍 Findings

### Overview
- Total offers analyzed: 3,089
- Segment breakdown: 🟢 26.2% Preferred, 🟡 47.1% Acceptable, 🔴 26.7% High Risk

### 📈 Correlation patterns
- Quality score and on-time delivery rate are the strongest positive drivers 
  of performance_score (r = 0.44 and 0.43)
- Defect rate and return rate are the strongest negative drivers 
  (r = -0.44 and -0.39)
- Price per unit shows almost no correlation with performance_score (r = 0.03) — 
  price alone is not a meaningful predictor of offer quality in this dataset

### 🚚 Delivery mode pattern
- On-time delivery, defect rate, and return rate are nearly identical across 
  Air, Road, and Sea delivery modes — delivery mode does not appear to be a 
  significant driver of offer performance

## ✅ Recommendations
- Prioritize offers with high `quality_score` and `on_time_delivery_rate`, 
  since these correlate most strongly with overall performance
- Do not use price as a standalone filter for offer quality — it shows no 
  meaningful relationship with performance in this data
- Delivery mode does not need special weighting in future scoring models, 
  given the near-identical performance across modes

## 🛠️ Tools used
- Python, pandas, matplotlib, seaborn
- Jupyter Notebook (VS Code)

