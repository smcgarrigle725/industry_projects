# industry_projects

Data science and data engineering projects using public industry datasets across healthcare, finance, insurance, public health, and product analytics. Each project is scoped around a realistic business problem and demonstrates the full workflow from data ingestion through modelling, evaluation, and communication of results.

---

## Projects

| Folder | Domain | Problem | Methods | Dataset |
|---|---|---|---|---|
| `hospital_readmission_prediction/` | Healthcare | 30-day readmission risk modelling | Logistic regression, random forest, tidymodels, calibration curves, feature importance | Diabetes 130-US Hospitals (UCI) |
| `cancer_survival_analysis/` | Healthcare | Patient survival and competing risks after cancer diagnosis | Kaplan-Meier, Cox PH, competing risks, time-varying covariates | SEER / TCGA clinical data |
| `credit_default_modelling/` | Finance | Credit default risk scoring and threshold optimisation | Logistic regression, regularised regression, ROC/AUC, class imbalance | German Credit (UCI) |
| `financial_time_series_forecasting/` | Finance | Commodity and index price forecasting with model comparison | ARIMA, ETS, Prophet, rolling-origin CV, forecast evaluation | quantmod / tidyquant public data |
| `claims_frequency_severity/` | Insurance | Motor insurance pure premium modelling | Poisson/NB frequency, Gamma/Tweedie severity, combined pure premium | French MTPL (CASdatasets) |
| `customer_churn_survival/` | Insurance | Policy lapse modelling as a survival problem | Cox PH, accelerated failure time, classification comparison | IBM Telco Churn (Kaggle) |
| `infectious_disease_surveillance/` | Public Health | Seasonal disease surveillance and intervention detection | Time series decomposition, seasonal ARIMA, interrupted time series | CDC / WHO influenza surveillance |
| `environmental_health_exposure/` | Public Health | Environmental exposure and health outcome modelling | Spatial regression, mixed models, confounder adjustment | EPA air/water quality public data |
| `user_retention_funnel_analysis/` | Product Analytics | Cohort retention and funnel drop-off analysis | Cohort tables, retention curves, funnel analysis, logistic regression | Online Retail II (UCI) |
| `ab_test_evaluation/` | Product Analytics | End-to-end A/B test design, analysis, and segmentation | Power analysis, frequentist and Bayesian evaluation, novelty effect detection | Udacity A/B Testing dataset |

---

## Domains

**Healthcare** — patient outcome prediction, survival analysis, clinical risk scoring  
**Finance** — credit risk modelling, time series forecasting, default prediction  
**Insurance** — claims frequency and severity modelling, policy lapse and retention  
**Public Health** — disease surveillance, environmental exposure, spatial health outcomes  
**Product Analytics** — funnel optimisation, cohort retention, experimentation and A/B testing

---

## Structure

Each project folder follows a consistent layout:

```
project_name/
├── README.md          # problem statement, approach, results, business framing
├── data/              # data sources, download instructions (no raw data committed)
├── notebooks/         # exploratory analysis and modelling (Jupyter or R Markdown)
├── src/               # reusable scripts and modules
└── outputs/           # figures, tables, model summaries
```

---

*Part of a broader portfolio. See also:*  
*[ecological_data_science](https://github.com/samantha-mcgarrigle/ecological_data_science) · [r_methods_library](https://github.com/samantha-mcgarrigle/r_methods_library) · [python_methods_library](https://github.com/samantha-mcgarrigle/python_methods_library) · [sql_methods_library](https://github.com/samantha-mcgarrigle/sql_methods_library)*