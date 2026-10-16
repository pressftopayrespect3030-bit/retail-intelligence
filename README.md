# Retail Intelligence

End-to-end Data Science project based on the **Online Retail II** dataset. The goal is to turn transaction history into three useful outputs: repeat purchase probabilities, sales forecasts, and product recommendations.

## Project goals

1. Predict whether a customer will place a new product order within the next 90 days.
2. Forecast sales over a defined future horizon.
3. Recommend products using past orders.

## Data

Source: Online Retail II, UCI Machine Learning Repository. Place the Excel workbook at `data/raw/online_retail_II.xlsx` before running the notebooks. Keep the raw dataset local and exclude it from Git.

The workbook contains two sheets, `Year 2009-2010` and `Year 2010-2011`. Their periods overlap in December 2010. In the customer analysis, 1,088 invoices were confirmed to have identical rows in both sheets; 22,523 repeated rows from the first sheet were removed before calculating customer metrics.

## Current progress

| Notebook | Scope | Status |
| --- | --- | --- |
| `notebooks/01_eda.ipynb` | Initial exploration of the `Year 2010-2011` sheet, including cancellations, missing customer IDs, special stock codes, and inventory adjustments | Complete |
| `notebooks/02_customer_behavior.ipynb` | Combine both sheets, define product purchases, build order and customer summaries, study repeat orders and RFM | Complete |

The customer analysis keeps rows with a known customer, positive quantity and price, and a non-cancelled invoice. It excludes selected non-product codes (postage, fees, adjustments, discounts, manual entries without identifiable products, and test records) from product-order metrics. These are task-specific definitions; they do not label every excluded record as erroneous.

### Customer behavior findings

- 36,594 product orders from 5,852 customers after the current filters.
- 72.4% of observed customers placed at least two product orders during the full data period.
- The median observed gap between consecutive orders is approximately 25 days.
- Among 5,256 customers with a full 90-day observation window after their first order, 46.6% placed a second product order within 90 days.
- At the descriptive analysis date, median RFM values are 96 days since the last order, 3 orders, and 860.26 in positive product-line amounts.

The monetary metrics are sums of positive product lines, **not net revenue**: later returns have not been subtracted. The 72.4% repeat-customer share covers the entire observed period and is **not** the 90-day classification target.

## Next stages

1. **Repeat purchase classification:** create customer snapshots at historical cutoff dates. Compute features only from orders available before each cutoff and label a purchase in the following 90 days. Use chronological validation and compare models with a simple baseline.
2. **Sales forecasting:** define a consistent sales measure, build a daily or weekly series, compare naive forecasts with models, and evaluate on later periods.
3. **Recommendations:** create item and customer interaction histories, compare popularity and item-based baselines, and evaluate recommendations on later orders.
4. **Reproducible application:** move reusable data preparation into `src/`, add training and prediction entry points, save model artifacts, and provide a small interface or API.

## Tools

Python and pandas are used in the current notebooks. scikit-learn, CatBoost, SQL, FastAPI, Streamlit, and Docker are candidates for later stages; they are not yet implemented in this project.
