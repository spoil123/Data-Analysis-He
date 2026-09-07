---
AIGC:
    Label: "1"
    ContentProducer: 001191440300708461136T1XGW3
    ProduceID: 91f7161bc71e2c663036f04b43335e19_f7574ad4aa9711f1b128525400f8a581
    ReservedCode1: 7ufS6iOp/6jhVLZiQDSSLkRL5yDUg4qMeYq2CrRIst4naW/dnIJPsi8ZVq63foPUNzmh8YAug/FHsa0p0wsWRE7v9i6N+q02Oconl9YwAZfFBzrkrvsviMqvjAc+DiHqeDSR4KNEjFGyf2vCCOoyq7tfKCtg/6WxJYKhO+RUhUXtPB4YoBCRCAptBbM=
    ContentPropagator: 001191440300708461136T1XGW3
    PropagateID: 91f7161bc71e2c663036f04b43335e19_f7574ad4aa9711f1b128525400f8a581
    ReservedCode2: 7ufS6iOp/6jhVLZiQDSSLkRL5yDUg4qMeYq2CrRIst4naW/dnIJPsi8ZVq63foPUNzmh8YAug/FHsa0p0wsWRE7v9i6N+q02Oconl9YwAZfFBzrkrvsviMqvjAc+DiHqeDSR4KNEjFGyf2vCCOoyq7tfKCtg/6WxJYKhO+RUhUXtPB4YoBCRCAptBbM=
---

# Data Analysis - E-commerce User Value Segmentation & Precision Marketing

An end-to-end data analysis project that segments e-commerce users by **value** using an extended **RFM-I (Recency, Frequency, Monetary, Intent) framework**, and derives **precision marketing strategies** with measurable ROI estimates.

This project demonstrates a complete data science workflow: data quality inspection → feature engineering → exploratory data analysis (EDA) → user segmentation → segment profiling → campaign ROI estimation.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Core Method: The RFM-I Model](#core-method-the-rfm-i-model)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [How to Run](#how-to-run)
- [Key Findings](#key-findings)
- [Data Source & Privacy](#data-source--privacy)
- [License](#license)

---

## Project Overview

Customer value segmentation is a fundamental problem in e-commerce marketing. Instead of treating all users the same, businesses must identify **who are the most valuable customers** and **how to allocate marketing budgets** to maximize return.

This project takes a standard transactional dataset (1,000 users × 14 features) and:

1. Audits data quality (missing values, outliers).
2. Engineers an **extended RFM-I feature set**, going beyond the classic RFM by adding user *Intent* (engagement depth), *Friction* (purchase resistance), *Loyalty* and *Income level*.
3. Performs exploratory data analysis to understand user behaviour.
4. Segments users into distinct value groups.
5. Profiles each segment with radar charts and distribution plots.
6. Compares **traditional RFM vs. optimised RFM-I** marketing strategies via ROI simulation.

The results show that the optimised RFM-I strategy significantly outperforms the traditional approach in marginal ROI (**33.5% vs 4.0%**), while spending less.

---

## Core Method: The RFM-I Model

### Classic RFM

| Metric | Meaning |
|--------|---------|
| **R** (Recency) | How recently a user made a purchase |
| **F** (Frequency) | How often a user purchases |
| **M** (Monetary) | How much a user spends |

### Extended RFM-I Features

To capture behaviour that classic RFM misses, additional engineered features are introduced:

| Feature | Meaning | Construction |
|---------|---------|--------------|
| **I_Score** | Intent depth — how engaged the user is | `0.5 * Time_Spent_Norm + 0.5 * Pages_Viewed_Norm` (min-max normalised) |
| **Friction** | Purchase resistance | `Pages_Viewed / (Purchase_Frequency + 1)` |
| **L_Score** | Loyalty / activity connection | Rule-based from newsletter subscription & recency of login (scale 1–3) |
| **Income_Level** | Purchasing power background | Quantile-based (Low / Medium / High by 33% & 66% percentiles) |
| **Interest_Match** | User-product category fit | `Interests == Product_Category_Preference` |

Users are then **scored and segmented** by combining these dimensions, and each segment receives a tailored marketing strategy.

---

## Tech Stack

- **Python 3** (Jupyter Notebook environment)
- **pandas** — data loading, cleaning, feature engineering
- **numpy** — numerical computation
- **matplotlib** — EDA visualisation, radar charts, distribution plots
- **openpyxl** — reading `.xlsx` input data

---

## Project Structure

```
Data-Analysis-He/
├── Rfm_User_Value_Segmentation.ipynb          # Main project notebook
├── Rfm_User_Value_Segmentation_Handcrafted.ipynb  # Re-implementation from scratch
├── Project_Deep_Dive.docx                     # Detailed project interpretation (supporting doc)
├── README.md                                  # This file
├── requirements.txt                           # Python dependencies
├── .gitignore                                 # Ignore data / temp files for privacy
└── LICENSE                                    # MIT License
```

### File Descriptions

| File | Description |
|------|-------------|
| `Rfm_User_Value_Segmentation.ipynb` | The complete analysis pipeline: quality checks → feature engineering → EDA → segmentation → profiling → ROI estimation |
| `Rfm_User_Value_Segmentation_Handcrafted.ipynb` | An independent re-implementation of the same analysis, written from scratch to verify the methodology |
| `Project_Deep_Dive.docx` | A deep-dive write-up explaining the business context, method rationale, and results |

---

## How to Run

### 1. Environment Setup

```bash
# (Recommended) create and activate a virtual environment
python -m venv venv
source venv/bin/activate        # Linux/macOS
# venv\Scripts\activate         # Windows

# Install dependencies
pip install -r requirements.txt
```

### 2. Prepare the Data

> **Note:** The original raw data is **not included** in this repository for privacy reasons (see [Data Source & Privacy](#data-source--privacy)).

The notebook expects an Excel file `user_personalized_features.xlsx` with the following schema (1,000 rows × 14 columns):

`User_ID, Age, Gender, Location, Income, Interests, Last_Login_Days_Ago, Purchase_Frequency, Average_Order_Value, Total_Spending, Product_Category_Preference, Time_Spent_on_Site_Minutes, Pages_Viewed, Newsletter_Subscription`

Place the file at the path specified in the notebook (currently `D:/工作/RFM项目/user_personalized_features.xlsx`) — **update the `input_path` variable** in the notebook to point to your local file.

### 3. Run

Open `Rfm_User_Value_Segmentation.ipynb` in Jupyter and execute all cells. Output charts are written to `output_dir`.

---

## Key Findings

1. **Clean, well-structured data.** The dataset (1,000 users × 14 features) had no missing values and no age outliers, allowing analysis to proceed without heavy cleaning.

2. **Diverse user behaviour.** I_Score (engagement depth) ranged from 0.00 to 98.83 and Friction from 0.10 to 49.00, confirming meaningful behavioural differences across users.

3. **Clear value segments.** Users were partitioned into distinct value segments (e.g. core / potential / low-value users), each with a distinctive RFM-I radar profile, enabling targeted treatment.

4. **Interest-match gap.** Initial `Interest_Match` (user interest vs. purchased category overlap) was only ~0%, highlighting a rich opportunity for personalised recommendation.

5. **Optimised RFM-I strategy beats classic RFM in ROI.** Simulated campaign ROI over a ¥10,000 budget:

   | Strategy | Target Users | Cost | Marginal ROI |
   |----------|--------------|------|--------------|
   | A: Traditional RFM | 200 | ¥2,000 | **4.0%** |
   | B: Optimised RFM-I | 159 (incl. 89 newly discovered potential users) | ¥1,590 (15.9% budget used) | **33.5%** |

   The optimised strategy both **cuts cost** and **unlocks high-potential users** that classic RFM overlooks.

---

## Data Source & Privacy

- The original data file (`user_personalized_features.xlsx`) and any raw CSV data are **not uploaded** to this repository to protect user privacy and business data.
- All personal identifiers in the analysis are **pseudo-anonymised** (users referred to as `#1`, `#2`, ...).
- To reproduce the work, use your own locally available dataset with the same schema, or generate a synthetic equivalent.

---

## License

This project is licensed under the [MIT License](LICENSE). Copyright (c) 2026 **He Langjie**.
*（内容由AI生成，仅供参考）*
