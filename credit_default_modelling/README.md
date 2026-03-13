# Credit Default Modelling

**Domain:** Finance  
**Dataset:** German Credit Dataset (UCI Machine Learning Repository)  
**Language:** R  

---

## Business Problem

Credit default prediction is one of the most established quantitative problems in financial services. Lenders need to assess the probability that an applicant will default on a loan, both to make individual lending decisions and to price credit risk appropriately across a portfolio. Regulatory frameworks (Basel III, IFRS 9) require banks to maintain internally validated probability-of-default (PD) models, making this a critical applied statistics problem in finance.

This project builds an interpretable credit default model, progressing from logistic regression through regularised approaches, with careful attention to model calibration, class imbalance, and threshold selection — all of which matter in a real lending context.

---

## Dataset

The German Credit dataset classifies 1,000 loan applicants as good or bad credit risks based on 20 predictor variables covering financial history, loan characteristics, and personal attributes.

**Key variables:**
- Financial: checking account status, credit history, savings account, credit amount, duration
- Loan: purpose, instalment rate, other debtors/guarantors, existing credits
- Personal: employment duration, personal status, age, housing, job
- Outcome: credit risk (good / bad)

**Source:** [UCI Machine Learning Repository — Statlog German Credit Data](https://archive.ics.uci.edu/dataset/144/statlog+german+credit+data)

---

## Approach

### 1. Data ingestion and cleaning
- Load and decode categorical variable codes (dataset uses numeric codes for categories)
- Inspect class balance: proportion of good vs. bad credit
- Cost-sensitive framing: misclassifying a bad risk as good is more costly than the reverse (standard in credit)

### 2. Exploratory data analysis
- Default rate by key predictors (checking account status, credit history, purpose)
- Distribution of credit amount and duration by outcome
- Correlation and association checks among predictors

### 3. Modelling
- Logistic regression: full model, odds ratios, confidence intervals
- Regularised regression: LASSO and elastic net via `glmnet` for variable selection
- Cost-sensitive evaluation: define asymmetric misclassification cost matrix
- Cross-validated performance comparison

### 4. Class imbalance and threshold optimisation
- Assess impact of class imbalance on default predictions
- Precision-recall tradeoff across thresholds
- Optimal threshold selection under the asymmetric cost structure
- SMOTE as an alternative to threshold adjustment

### 5. Calibration and model validation
- Calibration curve: are predicted probabilities well-calibrated?
- Platt scaling if calibration is poor
- Final model performance: ROC-AUC, Brier score, Gini coefficient (standard in credit)

### 6. Results communication
- Coefficient plot with confidence intervals (logistic model)
- ROC curve with AUC
- Calibration plot
- Confusion matrix under selected threshold with cost framing

---

## Methods

| Method | Purpose |
|---|---|
| Logistic regression | Interpretable baseline; odds ratios for risk factors |
| LASSO / elastic net | Variable selection; regularisation for correlated predictors |
| Cost-sensitive classification | Asymmetric misclassification costs (credit context) |
| Threshold optimisation | Operational decision threshold under cost constraints |
| Calibration curves | Reliability of predicted default probabilities |
| Gini coefficient | Industry-standard discrimination metric for credit models |

---

## Key Packages

`tidymodels`, `glmnet`, `probably`, `themis`, `ggplot2`, `broom`

---

## Business Framing

> *"What is the probability that this applicant will default, and at what threshold should we decline a loan application given the relative costs of approval errors?"*

Results are framed around lending operations: a scorecard-style summary of the most important risk factors, calibrated probability estimates, and an explicit cost-benefit analysis of the recommended classification threshold.

---

*industry_projects - Samantha McGarrigle*
