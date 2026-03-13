# Cancer Survival Analysis

**Domain:** Healthcare  
**Dataset:** SEER Breast Cancer Dataset (National Cancer Institute)  
**Language:** R  

---

## Business Problem

Survival analysis is a cornerstone of clinical oncology — understanding how long patients survive after diagnosis, which factors predict survival, and how competing causes of death (cancer vs. other causes) affect interpretation. These questions are central to clinical trial design, treatment efficacy evaluation, and resource allocation in oncology units.

This project applies a full survival analysis workflow to breast cancer patient data, progressing from non-parametric survival curves through semi-parametric Cox regression to competing risks modelling. The framing mirrors what a biostatistician or clinical data scientist would produce to support a clinical team or regulatory submission.

---

## Dataset

The SEER (Surveillance, Epidemiology, and End Results) Breast Cancer dataset contains patient-level records from the National Cancer Institute's cancer registry. The version used here is a cleaned public extract commonly used for survival analysis demonstration.

**Key variables:**
- Demographics: age, race
- Tumour characteristics: stage (T, N, M), grade, tumour size, regional node examination
- Treatment: oestrogen status, progesterone status
- Survival: survival months, vital status (alive / dead)

**Source:** [SEER Breast Cancer Data — NCI / Kaggle public extract](https://www.kaggle.com/datasets/reihanenamdari/breast-cancer)

---

## Approach

### 1. Data ingestion and cleaning
- Load and inspect data; verify survival time and event indicator coding
- Handle missing values in tumour stage and node variables
- Create clean survival object (`Surv()`)

### 2. Exploratory survival analysis
- Overall Kaplan-Meier survival curve with confidence intervals
- Stratified KM curves by stage, grade, and oestrogen status
- Log-rank tests for group differences
- Median survival and restricted mean survival time (RMST)

### 3. Cox proportional hazards regression
- Univariable Cox models for each predictor
- Multivariable Cox model with clinically motivated variable selection
- Proportional hazards assumption: Schoenfeld residuals, `cox.zph()`
- Hazard ratio forest plot

### 4. Competing risks analysis
- Cause-specific hazards vs. subdistribution (Fine-Gray) hazards
- Cumulative incidence functions by cause of death
- Fine-Gray model for cancer-specific mortality

### 5. Time-varying covariates (extension)
- Demonstrate `tmerge()` episode splitting for a time-varying predictor
- Comparison of naive vs. time-varying Cox estimates

### 6. Results communication
- KM plot with risk table (ggsurvplot)
- Forest plot of adjusted hazard ratios
- Cumulative incidence curves for competing risks
- Clinical interpretation of findings

---

## Methods

| Method | Purpose |
|---|---|
| Kaplan-Meier estimator | Non-parametric survival curves by group |
| Log-rank test | Group survival comparison |
| Cox proportional hazards | Semi-parametric multivariable survival model |
| Schoenfeld residuals | PH assumption testing |
| Fine-Gray model | Competing risks — subdistribution hazards |
| Restricted mean survival time | Summary measure robust to crossing curves |
| Time-varying covariates | Handle predictors that change over follow-up |

---

## Key Packages

`survival`, `survminer`, `cmprsk`, `ggplot2`, `broom`, `gtsummary`

---

## Business Framing

> *"Which patient and tumour characteristics predict survival, and how does cause-specific mortality differ from all-cause mortality in this population?"*

Results are framed around clinical utility: adjusted hazard ratios for treatment-relevant predictors, median survival by stage, and a competing risks breakdown that clarifies whether observed mortality differences are driven by cancer or other causes.

---

*industry_projects - Samantha McGarrigle*
