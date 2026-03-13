# Customer Churn and Policy Lapse Modelling

**Domain:** Insurance  
**Dataset:** IBM Telco Customer Churn Dataset (Kaggle)  
**Language:** R  

---

## Business Problem

Customer retention is a core challenge across insurance, telecommunications, and financial services. In insurance specifically, policy lapse — a customer cancelling or not renewing their policy — directly erodes the premium base and undermines the actuarial assumptions that pricing models depend on. Understanding *who* is likely to lapse and *when* enables targeted retention interventions at the right point in the customer lifecycle.

This project frames churn as a survival problem rather than a simple binary classification, which is the more realistic formulation: the question is not just *whether* a customer churns but *how long* they stay, and what accelerates or delays departure. Both survival and classification approaches are implemented and compared.

---

## Dataset

The IBM Telco Customer Churn dataset is a widely used public dataset containing customer-level records from a telecommunications company. While the source domain is telecom, the structure — time-based tenure, contract type, service features, and a binary churn indicator — maps directly onto insurance policy lapse modelling.

**Key variables:**
- Customer: tenure (months), gender, senior citizen status, partner, dependents
- Contract: contract type (month-to-month / one-year / two-year), paperless billing, payment method
- Services: internet service, phone service, online security, tech support, streaming
- Financial: monthly charges, total charges
- Outcome: churn (yes / no) + tenure as time-to-event

**Source:** [IBM Telco Customer Churn — Kaggle](https://www.kaggle.com/datasets/blastchar/telco-customer-churn)

---

## Approach

### 1. Data ingestion and cleaning
- Load dataset; inspect churn rate and tenure distribution
- Handle missing values in TotalCharges
- Construct survival object: `Surv(tenure, churn_event)`
- Encode contract type and payment method as ordered factors where appropriate

### 2. Exploratory analysis
- Churn rate by contract type, payment method, and service bundle
- Tenure distribution by churned vs. retained customers
- Monthly charges and total charges by churn status

### 3. Survival analysis
- Kaplan-Meier curves by contract type and key predictors
- Log-rank tests for group differences
- Median tenure to churn by segment

### 4. Cox proportional hazards model
- Univariable Cox models for each predictor
- Multivariable Cox model: adjusted hazard ratios
- PH assumption testing via Schoenfeld residuals
- Hazard ratio forest plot

### 5. Accelerated failure time model
- Weibull AFT model as parametric alternative
- Time ratio interpretation: which factors *accelerate* time to churn?
- Comparison of Cox (semi-parametric) vs. Weibull AFT (parametric)

### 6. Classification comparison
- Logistic regression and random forest on binary churn outcome
- ROC-AUC comparison: survival model vs. classification model
- Discuss the additional insight survival framing provides (timing, not just probability)

### 7. Results communication
- KM curves with risk tables by contract type
- Forest plot of adjusted hazard ratios
- Predicted survival curves for customer profiles
- Retention intervention framing: when and who to target

---

## Methods

| Method | Purpose |
|---|---|
| Kaplan-Meier estimator | Non-parametric survival curves by segment |
| Log-rank test | Group survival comparison |
| Cox proportional hazards | Semi-parametric multivariable churn model |
| Weibull AFT model | Parametric survival model; time ratio interpretation |
| Logistic regression | Binary classification baseline for comparison |
| Random forest | Predictive classification comparison |

---

## Key Packages

`survival`, `survminer`, `ggplot2`, `broom`, `tidymodels`, `ranger`, `gtsummary`

---

## Business Framing

> *"Which customers are most at risk of lapsing, when in their tenure is that risk highest, and what contract or service features are the strongest protective factors?"*

Results are framed around retention strategy: a ranked list of at-risk customer segments, the timing of peak churn risk by contract type, and a predicted survival curve for a new customer profile that a retention team could act on.

---

*industry_projects - Samantha McGarrigle*
