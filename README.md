# 🛍️ Customer Segmentation Analysis using RFM

> *"We were sending the same email to our best customer and someone who hadn't shopped in a year. This project fixed that."*

---

## 📌 The Business Problem

A retail company with **2,240 customers** was running 5 marketing campaigns — but sending every offer to every customer regardless of their behaviour. The result?

- 💸 Wasted marketing budget on disengaged customers
- 📉 Low campaign conversion rates (as low as 1%)
- 😤 Loyal, high-value customers receiving the same generic offers as one-time buyers

**The question this project answers:**
> *Who are our customers really — and what's the smartest way to reach each group?*

---

## 🎯 What This Project Does

This end-to-end analysis uses the **RFM (Recency, Frequency, Monetary)** framework to:

1. **Segment** 2,240 customers into meaningful behavioural groups
2. **Validate** that those segments are statistically real (not just made-up labels)
3. **Analyse** which of the 5 campaigns worked for which customer groups
4. **Deliver** quantified, revenue-backed recommendations for the marketing team

---

## 🗂️ Project Structure

```
customer-segmentation/
│
├── Customer_Segmentation.ipynb   # Main analysis notebook
├── marketing_campaign.csv        # Dataset (download from Kaggle)
└── README.md                     # You are here
```

---

## 📦 Dataset

**Source:** [UCI / Kaggle — Customer Personality Analysis](https://www.kaggle.com/datasets/imakash3011/customer-personality-analysis)

A real-world CRM dataset from a retail company with 29 columns covering customer demographics, 2-year purchase history, channel behaviour, and campaign responses.

| Column Group | Examples | What It Captures |
|---|---|---|
| Demographics | `Year_Birth`, `Income`, `Marital_Status` | Who the customer is |
| Spend | `MntWines`, `MntMeatProducts`, `MntGoldProds` | How much spent per category (2 years) |
| Channels | `NumWebPurchases`, `NumStorePurchases` | Where they buy |
| Campaigns | `AcceptedCmp1` – `AcceptedCmp5` | Whether they **converted** on each campaign |

> **What does "AcceptedCmp = 1" mean?** It means the customer received the offer **and made a purchase** as a direct result — a purchase conversion event, not just an open or click.

### 🏷️ Campaign Index

| Campaign | Name | Description |
|---|---|---|
| `AcceptedCmp1` | **Reactivation Drive** | Win-back offer sent to lapsed customers |
| `AcceptedCmp2` | **New Arrivals Push** | Promoted new product lines via email/catalogue |
| `AcceptedCmp3` | **Seasonal Promotion** | Time-limited discounts across all categories |
| `AcceptedCmp4` | **Loyalty Reward** | Exclusive deals for repeat buyers |
| `AcceptedCmp5` | **Premium Upsell** | Premium product bundles for high spenders |

---

## 🔬 Methodology

### Step 1 — Data Cleaning
- Removed 24 null income records (~1% of data)
- Detected and removed a known data entry error: one customer with `Income = $666,666` (24 standard deviations from the mean)
- Filtered out implausible ages (> 90 years)

### Step 2 — Feature Engineering
New features derived from raw columns:

| Feature | Formula | Purpose |
|---|---|---|
| `Total_Spent` | Sum of all `Mnt` columns | 2-year monetary value |
| `Total_Purchases` | Sum of all `Num` purchase columns | Overall transaction count |
| `Age` | `2024 - Year_Birth` | Customer age |
| `Tenure_Days` | Days from `Dt_Customer` to snapshot | How long they've been a customer |
| `Total_Conversions` | Sum of all `AcceptedCmp` columns | Total campaigns converted on |

### Step 3 — RFM Scoring

Each customer is scored **1–5** on three dimensions:

| Dimension | Measure | Scoring Direction |
|---|---|---|
| **R** — Recency | Days since last purchase | Lower days = more recent = **score 5** |
| **F** — Frequency | Total number of purchases | Higher purchases = **score 5** |
| **M** — Monetary | Total 2-year spend | Higher spend = **score 5** |

### Step 4 — Segmentation

Rule-based segments assigned from RFM scores:

| Segment | Criteria | Profile |
|---|---|---|
| 🏆 **Champions** | R≥4, F≥4, M≥4 | Recent, frequent, high spenders |
| 💚 **Loyal Customers** | R≥3, F≥3, M≥3 | Consistent and reliable |
| ⚠️ **At Risk** | R=1, F≥3 | High-value customers going quiet |
| 💰 **Big Spenders** | M≥4 | Spend a lot but infrequently |
| 😴 **Hibernating** | R≤2, F≤2 | Low engagement across the board |
| 🔔 **Needs Attention** | All others | Mid-range — could go either way |

### Step 5 — Validation

Two independent methods confirm the segments are real:

**ANOVA Test** — confirms that spend differences across segments are statistically significant (p < 0.05), not due to chance.

---

## 📊 Key Findings

### 💡 Customers
- Median age: **52 years** | Median income: **~$51,000**
- Most common marital status: **Married / Together**

### 💡 Spending
- **Wines dominate** — accounting for ~50% of all category spend
- Strong income–spending correlation: **r = 0.79** (high earners spend significantly more)

### 💡 Campaigns
- **Campaign 5 (Premium Upsell)** — highest overall conversion rate
- **Campaign 2 (New Arrivals Push)** — lowest performance, under 1.5% across all segments
- Champions convert at the highest rate on premium and loyalty campaigns
- At Risk customers respond best to the Reactivation Drive

### 💡 Revenue Distribution

| Segment | Revenue Share | Customer Share |
|---|---|---|
| Loyal Customers | ~35% | ~30% |
| Champions | ~25% | ~15% |
| At Risk | ~15% | ~12% |
| Big Spenders | ~10% | ~8% |

> Loyal Customers and Champions together represent ~45% of customers but drive ~60% of revenue.

---

## 💰 Recommendations & Revenue Estimates

| # | Action | Target Segment | Campaign | Estimated Impact |
|---|---|---|---|---|
| 1 | Premium upsell + VIP tier | Champions | Campaign 5 | **+$30K–$45K/yr** |
| 2 | Win-back sequence (30-day deadline) | At Risk | Campaign 1 | **+$50K–$70K recovered** |
| 3 | Tiered loyalty rewards | Loyal Customers | Campaign 4 | **+$60K–$90K/yr** |
| 4 | Seasonal conversion push | Promising / Needs Attention | Campaign 3 | **+$20K–$35K** |
| 5 | Pause & redesign | All | Campaign 2 | **1.5–2× ROI on reallocated budget** |
| 6 | Low-cost automated drip (3 emails) | Hibernating | Email only | **+$8K–$15K recovered** |

**Total potential uplift: ~$170K–$255K** *(conservative estimates based on segment size and historical conversion rates)*

---

## 🛠️ Tools & Libraries

| Library | Used For |
|---|---|
| `pandas` | Data loading, cleaning, feature engineering, groupby aggregations |
| `numpy` | Numerical operations, z-score outlier detection |
| `matplotlib` + `seaborn` | Static charts — distributions, heatmaps, bar charts |
| `plotly` | Interactive treemap for segment visualisation |
| `scipy.stats` | ANOVA test for statistical validation |
| `scikit-learn` | K-Means clustering, StandardScaler, silhouette scoring |

---


## 📁 What's Inside the Notebook

| Section | What You'll Find |
|---|---|
| 1. Setup | Imports, data loading |
| 2. Data Cleaning | Null handling, outlier removal, age filtering |
| 3. Feature Engineering | Total spend, purchases, tenure, age |
| 4. EDA | Demographics, spending patterns, income vs spend, campaign overview |
| 5. RFM Segmentation | Scoring, segment assignment, treemap visualisation |
| 6. Validation | ANOVA test |
| 7. Campaign Analysis | Conversion rate heatmap by segment |
| 8. Revenue Distribution | Customer share vs revenue share |
| 9. Recommendations | 6 data-backed actions with revenue estimates |

---

## 👩‍💻 About Me

**Monika Jhajhra** — Data Analyst | B.Sc. Mathematics & Statistics (Distinction)

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=flat&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/monika-jhajhra-2b8431214/)
[![GitHub](https://img.shields.io/badge/GitHub-Portfolio-181717?style=flat&logo=github&logoColor=white)](https://monika-jhajhra.github.io/Portfolio)

---

## 📄 License

This project is open source under the [MIT License](LICENSE).

Dataset: [CC0 Public Domain](https://creativecommons.org/publicdomain/zero/1.0/) via Kaggle.
