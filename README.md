# UK-sector-analysis
# 🏢 UK Company Health Scorer
### Predicting company failure using 627,000 real UK companies from Companies House

![Python](https://img.shields.io/badge/Python-3.12-blue?logo=python)
![Databricks](https://img.shields.io/badge/Databricks-enabled-red?logo=databricks)
![XGBoost](https://img.shields.io/badge/XGBoost-0.923_AUC-green)
![Data](https://img.shields.io/badge/Data-Companies_House_UK-lightgrey)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

---
##  Project Overview

This end-to-end data science project builds a **company health scoring system** using freely available UK government data from Companies House. The model predicts whether a company is likely to fail — and assigns every company a risk band from **High Risk** to **Low Risk** — using only publicly available filing information.

This type of model is directly applicable to:
- **Credit risk analysis** — should we lend to this company?
- **Fintech & investment** — is this supplier/partner financially stable?
- **Consulting** — which sectors are structurally at risk?
- **Insurance underwriting** — what is the risk profile of this business?

---

## Results

| Metric | Value |
|---|---|
| **ROC-AUC** | **0.923** |
| Model | XGBoost Classifier |
| Training data | 501,761 companies |
| Test data | 125,441 companies |
| Features | 14 engineered features |
| Class balance | 91.1% active / 8.9% failed |

### Risk Band Validation

| Risk Band | Companies | Actual Failure Rate |
|---|---|---|
| 🔴 High Risk | 54,188 | **81.6%** |
| 🟠 Elevated | 20,284 | 14.0% |
| 🟡 Moderate | 133,000 | 5.5% |
| 🟢 Low Risk | 419,730 | **1.5%** |

> The model separates High Risk from Low Risk companies with a **54x difference in actual failure rates** — validating that the scoring system is meaningful and actionable.

---

## 🔍 Key Findings

### 1. Sector Risk
Hospitality and transport are the highest-risk sectors in the UK:

| Sector | Failure Rate |
|---|---|
| Licensed restaurants | 21.5% |
| Freight transport by road | 17.9% |
| Public houses and bars | 17.7% |
| Take-away food shops | 17.4% |
| Construction of roads | 15.6% |

> Hospitality (restaurants + pubs + takeaways combined) fails at **3x the national average**.

### 2. Late Filing is the Strongest Warning Sign
- Companies filing accounts **more than 14 months late** are significantly more likely to fail
- **35% of all UK companies** are currently late filers — a systemic risk signal
- Late filing is the **#1 ranked feature** in the SHAP explainability analysis

### 3. Dormant Companies are Safer Than Expected
- Dormant company failure rate: **8.2%**
- Non-dormant failure rate: **9.0%**
- Dormant companies are often deliberately inactive shell or holding vehicles — not at operational risk

---

## Dataset

**Source:** [Companies House Free Data Products](https://download.companieshouse.gov.uk/en_output.html)

- Freely available, no login required
- Updated monthly
- ~850,000 UK companies per part file
- 55 columns including SIC codes, filing dates, company status, mortgage charges

**No paid APIs or subscriptions were used in this project.**

---

## Tech Stack

| Tool | Purpose |
|---|---|
| **Python 3.12** | Core language |
| **PySpark** | Loading and processing large CSV on Databricks |
| **Pandas** | Data manipulation and feature engineering |
| **XGBoost** | Gradient boosted tree classifier |
| **SHAP** | Model explainability |
| **Matplotlib / Seaborn** | Data visualisation |
| **Databricks** | Cloud notebook environment |
| **Companies House API** | Source data |

---

## Project Structure

```
uk-company-health-scorer/
├── notebooks/
│   ├── 01_load_data.ipynb          # Load CSV into Spark, convert to Pandas
│   ├── 02_eda.ipynb                # Exploratory analysis, sector dissolution rates
│   ├── 03_feature_engineering.ipynb # Date parsing, encoding, feature matrix
│   ├── 04_model_training.ipynb     # XGBoost training, evaluation, SHAP
│   └── 05_health_scoring.ipynb     # Risk band assignment and validation
├── charts/
│   ├── dissolution_by_sector.png   # Sector failure rates chart
│   ├── risk_band_validation.png    # Model validation chart
│   ├── shap_importance.png         # SHAP feature importance
│   └── shap_beeswarm.png           # SHAP beeswarm plot
└── README.md
```

---

## ⚙️ How to Reproduce

### 1. Get the data
Download the free Companies House bulk data:
```
https://download.companieshouse.gov.uk/BasicCompanyData-2026-04-01-part1_7.zip
```
Any single part file (~70MB) is sufficient to run the full pipeline.

### 2. Set up environment
```bash
pip install pandas xgboost shap matplotlib seaborn scikit-learn pyarrow
```

### 3. Run notebooks in order
```
01 → 02 → 03 → 04 → 05
```

### 4. On Databricks
Upload the CSV to DBFS and update the `FILE_PATH` variable in notebook 01:
```python
FILE_PATH = "dbfs:/FileStore/tables/BasicCompanyData-2026-04-01-part1_7.csv"
```

---

## 📈 Charts

### Sector Failure Rates
![Sector dissolution rates](charts/dissolution_by_sector.png)

### Risk Band Validation
![Risk band validation](charts/risk_band_validation.png)

---

##  What I Learned

- **Data quality matters more than model complexity** — fixing date overflows and column name spaces had more impact than hyperparameter tuning
- **Class imbalance handling** — using `scale_pos_weight` in XGBoost gave cleaner results than SMOTE for this dataset
- **SHAP explainability** is essential for any model used in a business context — knowing *why* a company is high risk is as important as the prediction itself
- **Dormant ≠ high risk** — my initial assumption was wrong, and documenting that finding honestly strengthens the analysis

---

## Future Work

- [ ] Add SHAP force plots for individual company explanations
- [ ] Incorporate financial accounts data (XBRL filings) for balance sheet features
- [ ] Build an interactive Streamlit dashboard for company lookup
- [ ] Train on all 7 part files (~5.9M companies) for a production-scale model
- [ ] Add NLP features from company name patterns using spaCy

---

## 👩‍💻 Author

Srishti — Aspiring Data Analyst based in the UK

🔗 [LinkedIn]([https://linkedin.com/in/your-profile](https://www.linkedin.com/in/srishti-dey/)) | 📧 your-srishti.workspace12@gmail.com

---

## 📄 Licence

This project is open source under the [MIT Licence](LICENSE).

Data sourced from Companies House, © Crown copyright, used under the [Open Government Licence](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/).
