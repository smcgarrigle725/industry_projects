# Hospital Readmission Prediction

**Domain:** Healthcare  
**Dataset:** Diabetes 130-US Hospitals 1999–2008 (UCI Machine Learning Repository)  
**Language:** R  

---

## Business Problem

Hospital readmissions within 30 days of discharge are a key quality and cost metric in healthcare. In the US, avoidable readmissions cost billions annually and are subject to financial penalties under the Hospital Readmissions Reduction Program (HRRP). The ability to identify high-risk patients at discharge enables targeted interventions — follow-up calls, care coordination, medication review — that reduce readmission rates and improve patient outcomes.

This project builds and evaluates a predictive model for 30-day readmission risk using structured clinical and administrative data from 130 US hospitals.

---

## Dataset

The Diabetes 130-US Hospitals dataset contains ~100,000 inpatient encounters for diabetic patients across 130 hospitals between 1999 and 2008. Each row represents a single hospital encounter.

**Key variables:**
- Demographics: age (binned), gender, race
- Admission details: admission type, discharge disposition, admission source
- Clinical: number of diagnoses, procedures, lab procedures, medications
- Diabetes-specific: HbA1c result, glucose serum test, insulin, change in diabetes medications
- Outcome: readmitted (no / <30 days / >30 days)

**Source:** [UCI Machine Learning Repository — Diabetes 130-US Hospitals](https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008)

---

## Approach

### 1. Data ingestion and cleaning
- Load and inspect raw data; document missingness patterns
- Handle `?` coded missing values
- Collapse readmission outcome to binary: readmitted within 30 days vs. not
- Encode categorical variables; handle high-cardinality features (e.g. ICD-9 diagnosis codes grouped to disease categories)

### 2. Exploratory data analysis
- Readmission rate by key variables (age, discharge disposition, number of medications)
- Missingness visualisation
- Class imbalance assessment

### 3. Modelling
- Baseline: logistic regression with clinically motivated predictors
- Extended: random forest via `ranger` within a `tidymodels` pipeline
- Class imbalance handling: SMOTE via `themis`
- Hyperparameter tuning: `tune_grid()` with cross-validation

### 4. Evaluation
- ROC-AUC, precision-recall curve, F1
- Calibration curve: are predicted probabilities reliable?
- Threshold selection: optimise for sensitivity vs. specificity depending on clinical framing
- Feature importance: permutation importance + SHAP values for top model

### 5. Results communication
- Summary table of model performance
- Calibration plot
- SHAP beeswarm plot of top predictors
- Business framing: what would a 5-point improvement in AUC mean operationally?

---

## Methods

| Method | Purpose |
|---|---|
| Logistic regression | Interpretable baseline; odds ratios for clinical variables |
| Random forest (`ranger`) | Primary predictive model |
| SMOTE | Class imbalance correction |
| Calibration curves | Assess reliability of predicted probabilities |
| SHAP values | Model explainability for clinical stakeholders |
| Cross-validation (`tidymodels`) | Unbiased performance estimation |

---

## Key Packages

`tidymodels`, `ranger`, `themis`, `probably`, `shapviz`, `ggplot2`, `naniar`

---

## Business Framing

> *"Which patients are most likely to be readmitted within 30 days, and what clinical factors are driving that risk?"*

Results are framed around operational deployment: a risk score at discharge, a ranked list of modifiable risk factors, and a recommended probability threshold for triggering a care coordination intervention.

---

*industry_projects - Samantha McGarrigle*
