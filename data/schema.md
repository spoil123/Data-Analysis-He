# Data Schema

Field dictionary for the input dataset used by both notebooks.

- **Dataset**: `user_personalized_features` (Kaggle: e-commerce user personalised features)
- **Size**: 1,000 users &times; 14 features
- **Role**: raw data is **not committed** to this repository for privacy reasons. Place your own copy under `data/` before running (see `README.md` &rarr; How to Run).
  - Main notebook reads: `data/user_personalized_features.xlsx`
  - Handcrafted notebook reads: `data/user_personalized_features.csv`

| Column | Type | Description | RFM-I mapping |
|--------|------|-------------|---------------|
| `User_ID` | text | Unique user identifier (pseudo-anonymised as `#1`, `#2`, ...) | — |
| `Age` | numeric | User age (18&ndash;64) | — |
| `Gender` | categorical | Gender (`Male` / `Female`) | — |
| `Location` | categorical | Area type (`Urban` / `Suburban` / `Rural`) | — |
| `Income` | numeric | Annual income (USD) | Income_Level feature |
| `Interests` | categorical | Interest segment (`Sports` / `Technology` / `Fashion` / `Travel` / `Food`) | Interest_Match |
| `Last_Login_Days_Ago` | numeric | Days since last login (smaller = more active) | **R** (Recency) |
| `Purchase_Frequency` | numeric | Purchase frequency | **F** (Frequency) |
| `Average_Order_Value` | numeric | Average order value | M context |
| `Total_Spending` | numeric | Total spending amount | **M** (Monetary) |
| `Product_Category_Preference` | categorical | Preferred product category (`Books` / `Electronics` / `Apparel` / `Health & Beauty` / `Home & Kitchen`) | Interest_Match |
| `Time_Spent_on_Site_Minutes` | numeric | Time spent on site (minutes) | I_Score (Intent) |
| `Pages_Viewed` | numeric | Pages viewed | I_Score / Friction |
| `Newsletter_Subscription` | boolean | Subscribed to e-mail marketing | L_Score (Loyalty) |

> **Note**: `Interests` and `Product_Category_Preference` use different category encodings, so a per-row direct comparison yields a very low match rate. This is explained in the feature-engineering step and is one of the key findings.
