# Customer Segmentation Using RFM Analysis

Segments customers from the **Online Retail II** dataset into behavioral groups (Champions, Loyal, Recent, At Risk, Others) using the RFM (Recency, Frequency, Monetary) framework, then translates each segment into a marketing recommendation.

## Project Overview

Customer segmentation helps businesses understand different purchasing behaviors and design targeted campaigns. This notebook computes an RFM score for every customer, buckets them into five segments, visualizes the distribution and revenue contribution of each, and closes with business recommendations and next steps.

## Requirements

- Python 3
- pandas
- numpy
- matplotlib
- seaborn

Install with:
```bash
pip install pandas numpy matplotlib seaborn
```

## Data

- **File expected:** `online_retail_II.csv`, placed in the same directory as the notebook.
- **Source:** UCI Machine Learning Repository / Kaggle "Online Retail II" dataset (UK-based online retailer, transactions from 2009–2011).
- **Key columns used:** `Invoice`, `Customer ID`, `InvoiceDate`, `Quantity`, `Price`.

## How to Run

1. Place `online_retail_II.csv` in the same folder as the notebook.
2. Open the notebook and run all cells top to bottom.
3. Run the final two analysis cells (`segment_summary` and `revenue_pct`) — their output feeds the numbers referenced in the closing "Conclusion & Key Insights" section, so re-run them any time the underlying data changes.

## Methodology

1. **Load & clean data** — drop nulls, remove cancelled orders (invoices starting with `"C"`), and filter out non-positive quantity/price rows.
2. **Feature engineering** — compute `TotalPrice` (Quantity × Price) and standardize column names (`CustomerID`, `InvoiceNo`, `UnitPrice`).
3. **RFM table** — for each `CustomerID`, calculate:
   - **Recency**: days since the customer's last purchase (relative to a snapshot date = max invoice date + 1 day)
   - **Frequency**: number of unique invoices
   - **Monetary**: total spend
4. **Scoring** — each of Recency, Frequency, and Monetary is split into quintiles (1–5) via `pd.qcut`, then concatenated into a single `RFM_Score` (e.g. `"555"`).
5. **Segmentation rules** (`segment_customer`):
   - `RFM_Score == "555"` → **Champions**
   - `F_Score >= 4` and `M_Score >= 4` → **Loyal Customers**
   - `R_Score >= 4` → **Recent Customers**
   - `R_Score <= 2` → **At Risk**
   - everything else → **Others**
6. **Visualization** — customer count by segment (bar chart), revenue contribution by segment (bar chart), and a Recency/Frequency/Monetary heatmap by segment.
7. **Summary tables** — segment-level customer counts, percentage share, and revenue share.

## Segments & Recommendations

| Segment | Description | Recommended Action |
|---|---|---|
| **Champions** | Purchase frequently, recently, and generate high revenue | VIP rewards, exclusive products, encourage referrals |
| **Loyal Customers** | Strong frequency and spend | Loyalty programs, personalized discounts |
| **Recent Customers** | New, purchased recently, need engagement | Follow-up offers, encourage a second purchase |
| **At Risk** | Haven't purchased recently | Re-engagement campaigns, limited-time discounts |
| **Others** | Doesn't clearly fit the above | Monitor and re-evaluate on next run |

## Sample Result (from the notebook's last executed run)

Segment sizes (customer counts):

| Segment | Customers |
|---|---|
| At Risk | 2,120 |
| Loyal Customers | 1,458 |
| Recent Customers | 1,064 |
| Others | 762 |
| Champions | 474 |

*Note: the revenue-share and percentage summary cells (`segment_summary`, `revenue_pct`) were not executed in the saved notebook — re-run them to populate exact percentages and revenue contribution per segment.*

## Key Takeaways

- **Champions** are typically the smallest group by count but drive a disproportionate share of revenue (classic 80/20 pattern) — protecting and rewarding them is a top priority.
- **At Risk** customers are a meaningful churn threat; timely win-back campaigns could recover revenue before they're lost for good.
- **Loyal Customers** are a stable base worth nurturing into future Champions.
- **Recent Customers** represent growth potential — the goal is converting a first purchase into a repeat habit.

## Recommended Next Steps

1. Launch a win-back campaign for the "At Risk" segment (time-limited discounts).
2. Build a VIP loyalty tier for "Champions" to boost retention and referrals.
3. Automate follow-up offers for "Recent Customers" after their first purchase.
4. Re-run this analysis quarterly (or monthly) to track segment migration and measure whether marketing actions are moving customers toward higher-value segments.


## 👩‍💻 Author

**Shrouk Ahmed**
