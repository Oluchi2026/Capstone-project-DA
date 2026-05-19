# Section A Template (30 Marks) - Oluchi Frances

## 1) Executive Summary (10 marks)

### Business context
As Business Manager in a food company, I am accountable for growth quality, customer retention, and sales predictability. Recent volatility in customer purchase behavior requires a more targeted strategy grounded in predictive and segmentation analytics.

### Research question
Which customer segments are most likely to reduce or sustain purchases, and what data-driven interventions can improve retention and revenue growth?

### Three key findings (with numbers)
1. [Model metric: AUC/accuracy/precision for churn-risk prediction]
2. [Cluster insight: segment sizes and revenue/churn differential]
3. [Forecast insight: expected short-term demand trend]

### One recommendation (quantified)
[Segment-specific retention campaign] targeted at [segment] is projected to improve repeat-purchase rate by [X%] and increase monthly revenue by [Y%].

## 2) Professional Disclosure (10 marks)

**Job title:** Business Manager
**Organisation sector:** Food manufacturing and distribution — consumer packaged goods sold through distributors and retailers across Eastern, Northern, and Southern Nigeria

### Technique 1 — Classification Model
As Business Manager, my most operationally urgent question is which customers are at risk of reducing or stopping purchases over the next 12 months. With three years of transaction history covering 8,017 customer-item-month records, I have sufficient data to train a classification model that predicts each customer's trajectory — specifically whether their purchase volume will grow, remain stable, or decline. This model directly supports my decision on where to deploy sales representative time, promotional budgets, and distributor incentive programmes. Rather than relying on the sales team's subjective assessment of which customers are "loyal", I can now present a probability-ranked list of at-risk customers at the monthly commercial review and direct intervention resources accordingly.

**Alternative considered:** Descriptive customer profiling using average purchase value and frequency. Rejected because static profiling cannot distinguish customers on a declining trajectory from customers who are seasonally low but structurally loyal.

**Limitation:** Classification model quality depends on how the target variable (grow / stable / decline) is defined. If the threshold between "growing" and "stable" is set arbitrarily, the model will reflect that arbitrary boundary. The target definition must be grounded in a commercially meaningful revenue change threshold.

### Technique 2 — Model Evaluation & Explainability (SHAP)
My sales team and distributor partners are not data scientists. A prediction score without an explanation will not change their behaviour. SHAP (SHapley Additive exPlanations) values allow me to translate the model's output into language that the territory managers and distributors understand: "This distributor is flagged as at-risk because their purchase frequency has dropped from monthly to bi-monthly in the last two quarters, and they have not purchased from our premium product line in six months." These explanations will be the centrepiece of the monthly distributor review meeting where I present findings to the commercial director.

**Alternative considered:** Reporting only the confusion matrix, ROC curve, and AUC as summary model metrics. Rejected because aggregate metrics do not identify which specific customers to call this week or what intervention to propose.

**Limitation:** SHAP values are computed relative to the underlying model architecture. If the model is switched from logistic regression to XGBoost, SHAP values will shift even if the predictions are directionally similar.

### Technique 3 — Customer Segmentation (Clustering)
Our customer base of distributors and retailers is not homogeneous. A large East-zone distributor with monthly bulk purchases behaves completely differently from a small North-zone retailer buying weekly in small quantities. Applying clustering to the combined sales and survey data (purchase frequency, revenue, product mix, zone, income level, decision factors) reveals the natural customer archetypes that exist in the data. Each cluster maps to a different commercial strategy: high-value loyalists receive exclusive deals, at-risk mid-volume customers receive priority service calls, and low-engagement customers are targeted with trial promotions. This replaces the current informal zone-based grouping with a data-driven segmentation framework.

**Alternative considered:** Segmenting customers manually by a single variable such as average monthly revenue. Rejected because this ignores the interaction between purchase frequency, product mix breadth, and geographic zone that defines commercially meaningful customer types.

**Limitation:** K-means clustering requires the number of clusters to be specified in advance. The elbow method and silhouette score will justify the chosen k, but cluster stability will be validated across multiple seeds.

### Technique 4 — Dimensionality Reduction (PCA)
The customer survey collected 10 attributes per respondent (age group, town, monthly income, product familiarity, purchase frequency, decision factors, preference changes, and improvement suggestions). Many of these variables are likely correlated — for example, income level and product familiarity may move together. PCA reduces these 10 survey variables to the 2–3 components that explain the most variance, allowing me to plot the customer segments on a biplot and visually confirm that the clusters identified in Technique 3 are genuinely distinct rather than artefacts of how the features were scaled. This biplot will anchor the next distributor strategy presentation to the executive team.

**Alternative considered:** Plotting all 10 survey variables in a pairwise matrix. Rejected because a 10×10 scatterplot matrix is too complex for a management audience to interpret and obscures the dominant patterns.

**Limitation:** PCA components are linear combinations of input variables. If customer preferences relate non-linearly to purchasing behaviour, PCA will not fully capture the underlying structure. t-SNE will be explored as a supplementary check.

### Technique 5 — Time Series Analysis
Monthly sales target-setting at the company is currently done by applying a fixed percentage uplift to the prior year's number. The `period` field in the sales dataset provides 36 monthly data points from January 2023 to December 2025 — well above the 24-period minimum required for time series analysis. Decomposing the series into trend, seasonal, and residual components will reveal whether underlying growth is accelerating or decelerating, and whether there are consistent seasonal dip months that management should plan around. An ARIMA or ETS model forecast for Q1–Q2 2026 provides the finance and commercial teams with a statistically grounded revenue projection that replaces the manual uplift method.

**Alternative considered:** Simple period-over-period percentage comparison (e.g., Q4 2025 vs Q4 2024). Rejected because this approach cannot identify whether sales trends are structural or seasonal, and it cannot produce forward-looking forecasts with confidence intervals.

**Limitation:** ARIMA forecast accuracy assumes structural stability in the demand process. If a new competitor entered the market or a major distributor was lost during the period, the model's residuals will be large and the forecast horizon should be limited to 1–2 periods.

---

## 3) Data Collection & Sampling (10 marks)

- **Source A:** Customer survey — "Understanding Customer Preferences to Serve You Better" (primary data collection via structured questionnaire)
- **Source B:** Internal sales management system — three years of historical customer-level transaction records
- **Primary datasets used:**
  - `customer_survey_data.csv` — 190 respondents; 12 variables: Timestamp, age group, town, approximate monthly income, product line familiarity, products purchased in last 12 months, stated product preferences, preference change direction, reasons for change, purchase decision factors, purchase frequency, improvement suggestions
  - `sales_data_3yrs.csv` — 8,017 transaction records; 6 variables: Customer ID, Item ID, Qty, Stocking U/M, Amount, Period (month-year)
- **Collection method:**
  - Survey: A structured 10-question survey was designed by the student and administered via WhatsApp links distributed to active distributors and retailers by the company's sales representatives across multiple market zones. The survey was conducted over five days. Locations were grouped into three major zones — East, North West, and South — using a central geographic reference point for zone consistency.
  - Sales data: Exported directly from the company's internal sales management system by the student in their capacity as Business Manager. The export covers all customer transactions from January 2023 to December 2025, aggregated monthly at the customer-item level.
- **Period covered:**
  - Sales data: January 2023 – December 2025 (36 monthly periods; 3 full calendar years)
  - Survey data: Conducted during [INSERT SURVEY DATES — confirm month and year of administration]
- **Sample frame:**
  - Survey: Active distributors and retailers known to the company's sales team and reachable via WhatsApp across all three regional zones. The survey targets all customers who had purchased within the preceding 12 months.
  - Sales: All customer accounts with at least one recorded transaction during the three-year period. No accounts were manually excluded.
- **Sample size justification:**
  - Survey: 190 respondents exceed the 100-observation minimum for CS2 and satisfy the PCA rule of thumb (≥5 respondents per variable: 190/10 = 19). The sample also provides sufficient observations per cluster for a meaningful K-means segmentation.
  - Sales: 8,017 transaction records and 36 monthly time periods substantially exceed all CS2 minimums (200 obs for classification; 24 periods for time series). The classification model will use a customer-month aggregated version of the sales data to create one row per customer per month.
- **Ethical notes and confidentiality:** The survey was administered to voluntary participants who were informed that responses were for analytical and academic purposes. No personally identifiable customer information is included in either dataset — Customer IDs are anonymized internal codes assigned by the sales system. All raw numerical sales figures are confidential and will not be published in their disaggregated form in the public RPubs submission. The dataset is used exclusively for this academic exercise at Lagos Business School. [CONFIRM: Obtain and state explicit academic-use approval: "Use of this dataset was approved by [Name, Title] at [Company Name] for the purposes of this academic assessment."]
