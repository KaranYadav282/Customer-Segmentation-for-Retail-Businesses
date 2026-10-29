# 🛍️ Retail Customer Segmentation using RFM Analysis and K-Means Clustering

An end-to-end customer segmentation project that groups retail customers by their purchasing behaviour (**Recency, Frequency, Monetary value**) using **K-Means clustering**, and presents the results in an interactive **Streamlit** web application.

🔗 **Live App:** https://costumer-segmentation-d59y6iaubjqytg7fqgrfu6.streamlit.app/

![App Home](screenshots/00_streamlit_app_home.png)

---

## 👥 Team

| Name | Enrollment No. |
|---|---|
| Nishit Modi | 1ETBTD20251005 |
| Karan Sanjay Yadav | 1ETBTD20251009 |
| Dhoop Kumar Biswal | 1ETBTD20251014 |

---

## 📌 Problem Statement

Retailers often send the same offers to every customer. This wastes marketing budget and ignores the differences between loyal, occasional and inactive customers. This project automatically segments customers from raw transaction data, interprets each segment and recommends a marketing strategy for it.

## ✨ Features

- Upload any retail transaction CSV
- Automatic data cleaning and validation
- RFM feature engineering
- Log transformation + standardisation
- K-Means clustering with **elbow method** and **silhouette score**
- Automatic selection of the best K (highest silhouette score)
- Business-friendly segment names and marketing strategies
- PCA visualisation of segments
- Predict the segment of a **new customer**
- Download customer-level CSV, segment-profile CSV and Excel report

## 📊 Dataset

`data/online_retail.csv` — retail transactions from **1 Jan 2025 to 31 Dec 2025** (38,913 rows, 8 columns).

| Column | Description |
|---|---|
| `invoiceno` | Invoice number |
| `stockcode` | Product code |
| `description` | Product name |
| `quantity` | Units purchased (negative = return) |
| `invoicedate` | Transaction date |
| `unitprice` | Price per unit |
| `customerid` | Customer identifier |
| `country` | Country |

**Required columns for the app:** `CustomerID`, `InvoiceNo`, `InvoiceDate`, `Quantity`, `UnitPrice` (names are normalised automatically).

## ⚙️ Methodology

1. **Clean data** – remove missing CustomerID, duplicates, returns (Quantity ≤ 0) and invalid prices
2. **Build RFM** – Recency (days since last purchase, reference date = last date + 1), Frequency (distinct invoices), Monetary (total spend)
3. **Transform** – `log(1 + x)` then `StandardScaler`
4. **Choose K** – test K = 2…10, pick the highest silhouette score
5. **Cluster** – final K-Means (`random_state=42`, `n_init=20`)
6. **Name segments** – compare cluster averages with overall averages using rule thresholds
7. **Recommend** – attach a marketing strategy to each segment
8. **Predict** – apply the same transform, scaler and model to a new customer

## 🏆 Results

| Metric | Value |
|---|---|
| Valid transactions | 37,769 |
| Unique customers | 1,997 |
| Total revenue | 5,458,811.85 |
| Best K | 2 (silhouette = 0.4271) |

| Segment | Customers | Avg Recency (days) | Avg Frequency | Avg Monetary | Revenue share |
|---|---|---|---|---|---|
| High Value Customers | 1,088 | 54.66 | 12.57 | 4,463.29 | 89.0% |
| Low Engagement Customers | 909 | 178.99 | 2.65 | 663.09 | 11.0% |

**Insight:** about 54.5% of customers generate about 89% of revenue, so retaining the High Value group is critical. Monthly revenue also fell from about 824K in January to about 308K in December, which supports re-engagement campaigns for the Low Engagement group.

### Charts

| | |
|---|---|
| ![Monthly revenue](screenshots/01_monthly_revenue.png) | ![RFM distributions](screenshots/02_rfm_distributions.png) |
| ![Elbow](screenshots/05_elbow_method.png) | ![Silhouette](screenshots/06_silhouette_scores.png) |
| ![Segments](screenshots/07_segments_scatter.png) | ![PCA](screenshots/08_pca_segments.png) |
| ![Segment sizes](screenshots/09_segment_size_revenue.png) | ![RFM comparison](screenshots/10_segment_rfm_comparison.png) |

## 🎯 Marketing Strategies

| Segment | Strategy |
|---|---|
| High Value | Loyalty rewards, personalised recommendations, exclusive offers, early access |
| Low Engagement | Targeted promotions and low-cost re-engagement campaigns |
| Loyal | Loyalty points, repeat-purchase rewards, cross-selling |
| At Risk High-Value | Re-engagement campaigns and reminders based on past purchases |
| New or Occasional | Introductory offers and incentives for repeat purchase |
| Frequent Low-Spend | Bundles, cross-selling and volume-based offers |
| Inactive | Win-back campaigns and personalised offers |

## 🗂️ Project Structure

```
retail-customer-segmentation/
├── app.py                              # Streamlit web application
├── retail_customer_segmentation.py     # Command-line version (CSV outputs + charts)
├── requirements.txt                    # Dependencies
├── README.md
├── LICENSE
├── .gitignore
├── .streamlit/config.toml              # Streamlit configuration
├── data/online_retail.csv              # Dataset
├── outputs/                            # customer_segments.csv, segment_profile.csv, Excel report
└── screenshots/                        # App screenshot and result charts
```

## 🚀 How to Run Locally

```bash
# 1. Clone the repository
git clone https://github.com/<your-username>/<your-repo-name>.git
cd <your-repo-name>

# 2. (Optional) create a virtual environment
python -m venv venv
venv\Scripts\activate          # Windows
# source venv/bin/activate     # macOS / Linux

# 3. Install dependencies
pip install -r requirements.txt

# 4a. Run the web app, then upload data/online_retail.csv
streamlit run app.py

# 4b. OR run the script version (creates files in outputs/ and screenshots/)
python retail_customer_segmentation.py
```

## ☁️ Deploy on Streamlit Community Cloud

1. Push this repository to GitHub
2. Go to [share.streamlit.io](https://share.streamlit.io) and click **New app**
3. Select the repository, branch `main`, and main file `app.py`
4. Click **Deploy**, then upload `data/online_retail.csv` in the app

## 🛠️ Tech Stack

Python · Pandas · NumPy · Scikit-learn · Matplotlib · Seaborn · Streamlit · OpenPyXL

## 🔭 Future Scope

- Let the user choose K manually and compare other algorithms (hierarchical, DBSCAN, GMM)
- Add product-category and basket analysis
- Add churn and Customer Lifetime Value prediction

## 📄 License

This project is released under the terms in the [LICENSE](LICENSE) file.
