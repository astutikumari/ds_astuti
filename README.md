**Trader Sentiment & Profitability Analysis**


**Overview**

This repository contains an analysis of the relationship between trader profitability and market sentiment (Fear/Greed). The goal is to explore whether sentiment metrics can provide insight into trade outcomes and to build a baseline predictive pipeline.

**1) Input Data**

Trader data (trader_data.csv):
Executed trades with timestamps, size, price, PnL, account info, etc.

Sentiment data (fear_greed.csv):
Time-series Fear/Greed sentiment metrics.

#**Merged dataset:**
Trades were mapped to the most recent prior sentiment within a 3-day tolerance using merge_asof.
Resulting dataset used for feature engineering, EDA, and modeling.

**2) Processing & Feature Engineering**
Merged datasets (trader_data + fear_greed) to associate each trade with the most relevant sentiment.

Created target variable:
is_profitable = 1 if trade PnL > 0, else 0.

Engineered numeric features:
profit_per_size, risk_exposure, hour, weekday, daily_count, daily_mean_pnl.
Created sentiment label dummies (Extreme Greed, Greed, Fear, Neutral).

**3) Exploratory Data Analysis (EDA)**

Profit distribution: Heavy-tailed; mean influenced by outliers.
Average profit by sentiment: Greed trades have higher mean PnL than Fear trades.

Statistical tests:
t-test p-value: 0.04991 → Greed trades statistically higher PnL.
Mann-Whitney p-value: 0.00163 → confirms effect direction.

Plots are saved in /content/outputs:
closedpnl_dist.png
avg_profit_by_sentiment.png

**4) Baseline Models**

Goal: Predict is_profitable using trade and sentiment features.
Models: Random Forest, Logistic Regression (5-fold Stratified CV).
Features used: risk_exposure, hour, weekday, daily_count, daily_mean_pnl, profit_per_size, sentiment dummies.

Results (on training data — note: target leakage due to profit_per_size):
Model	ROC-AUC (CV mean ± std)
Random Forest	1.0 / 0.0
Logistic Regression	0.9788 / 0.0012

Top features (RF importance): profit_per_size, daily_mean_pnl, hour, daily_count.

**⚠️ Note: profit_per_size is derived from trade outcomes, introducing target leakage. For real predictive modeling, use only pre-trade features.**

**5) Outputs**

Processed dataset: /content/csv_files/trader_processed.csv
EDA plots: /content/outputs/*.png
PDF skeleton report: /content/outputs/ds_report_skeleton.pdf

**6) How to Use**

**Clone the repo:**
git clone <repo-url>

Place the two input CSVs in the repo root or update paths:
trader_data.csv
fear_greed.csv

Run the notebook to produce:
trader_processed.csv
EDA plots (closedpnl_dist.png, avg_profit_by_sentiment.png)
PDF report skeleton (ds_report_skeleton.pdf)

**7) Notes / Recommendations**

For real predictive modeling, remove target-leakage features like profit_per_size.
Use pre-trade features (hour, weekday, sentiment metrics) for realistic ML models.
Statistical tests indicate a small but significant effect of sentiment on profitability.

**8) Folder Structure**


repo/
├─ trader_data.csv
├─ fear_greed.csv
├─ csv_files/
│  └─ trader_processed.csv
├─ outputs/
│  ├─ closedpnl_dist.png
│  ├─ avg_profit_by_sentiment.png
│  └─ ds_report_skeleton.pdf
└─ README.md
