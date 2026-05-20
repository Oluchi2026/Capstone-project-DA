# Customer Analytics: Segmentation, Customer Value Classification, and Commercial Engagement Strategy

**Author:** OOLUCHUKWU FRANCISCA OBODO    
**Programme:** MBA — DATA ANALYTICS CAPSTONE PROJECT 
**Rendered output:** [`oluchi_capstone.html`](oluchi_capstone.html)

---

## Project Overview

This capstone analyses three years of sales transaction data (8,017 records, Jan 2023–Dec 2025) and 190 customer survey responses for a food manufacturing and distribution company operating across the East, North West, and South zones of Nigeria.

The analysis answers: **Which customer characteristics most reliably predict high-value customer status, and how can these insights be operationalised into a tiered commercial engagement strategy?**

---

## Analytical Techniques

| Section | Technique | Tools |
|---|---|---|
| 7 | Binary classification — high-value customer prediction | R `glm` + `rpart`, Python `LogisticRegression` + `DecisionTreeClassifier` |
| 8 | Model evaluation, SHAP feature importance, waterfall plot | `pROC`, `shap.LinearExplainer` |
| 9 | Customer segmentation (K-Means, elbow + silhouette) | R `kmeans`, Python `KMeans` |
| 10 | Dimensionality reduction — PCA biplot with cluster overlay | R `prcomp`, Python `sklearn.PCA` |
| 11 | Time series decomposition, ACF/PACF, ADF stationarity, ETS forecast | R `forecast` + `tseries`, Python `statsmodels` |

---

## Repository Structure

```
oluchi-capstone/
├── oluchi_capstone.qmd       # Main analysis document (Quarto)
├── oluchi_capstone.html      # Rendered HTML report
├── oluchi_report.css         # Report stylesheet
├── requirements.txt          # Python dependencies
├── README.md                 # This file
├── data/
│   ├── sales_data_3yrs.csv   # 3-year sales transactions (8,017 rows) — gitignored
│   └── customer_survey_data.csv  # 190 survey responses — gitignored
└── docs/
    ├── cs_review.md          # Case study review notes
    └── section_summary.md    # Section summaries
```

---

## Rendering the Report

### Prerequisites

**R packages** (installed automatically on first render if missing):

```r
install.packages(c(
  "readr", "dplyr", "tidyr", "tibble", "purrr", "forcats",
  "ggplot2", "lubridate", "scales", "broom", "knitr",
  "cluster", "forecast", "zoo", "rpart", "tseries",
  "ggcorrplot", "patchwork", "pROC", "kableExtra"
))
```

**Python packages:**

```bash
pip install -r requirements.txt
```

### Render command

```bash
quarto render oluchi_capstone.qmd
```

Or from the workspace root:

```bash
quarto render 'Oluchi Frances/oluchi_capstone.qmd'
```

> **Note:** Place `sales_data_3yrs.csv` and `customer_survey_data.csv` in the `data/` directory before rendering. These files are excluded from version control by `.gitignore`.

---

## Key Findings

1. **Classification:** Transaction frequency and product breadth are the strongest predictors of high-value customer status. Logistic regression and Decision Tree models are compared side-by-side; deployment recommendation favours logistic regression for interpretability unless the tree produces materially higher recall (≥5 pp).
2. **Segmentation:** K-Means clustering (k confirmed by silhouette scoring) identifies commercially distinct customer archetypes — Premium Accounts, Core Buyers, Occasional Buyers, and Reactivation Targets — each warranting a differentiated commercial playbook.
3. **PCA:** First two principal components explain the majority of customer variation, separating clusters cleanly in PC1–PC2 space. PC1 represents revenue scale; PC2 represents engagement depth.
4. **Time series:** Seasonal decomposition reveals meaningful trend and seasonal components. ADF stationarity test and ACF/PACF diagnostics inform ARIMA/ETS model selection. ETS forecast provides the 6-month revenue planning baseline.

---

## Data Governance

- Sales customer IDs are internal codes; no individual or business name is published.
- Survey data was collected voluntarily for academic purposes; no PII beyond geographic zone is retained.
- Survey and sales datasets share no common key and are analysed independently.
- Data is held locally and used exclusively for this academic submission.

---

## Dependencies

| Language | Version |
|---|---|
| R | ≥ 4.3 |
| Python | ≥ 3.9 |
| Quarto | ≥ 1.4 |

---

## References

- Adi, B. (2026). *AI-powered business analytics*. Lagos Business School / markanalytics.online.
- R Core Team (2024). *R: A language and environment for statistical computing*.
- Lundberg, S. M., & Lee, S. I. (2017). A unified approach to interpreting model predictions. *NeurIPS 30*.
