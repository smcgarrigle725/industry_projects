# Environmental Health and Exposure Modelling

**Domain:** Public Health  
**Dataset:** EPA Air Quality System (AQS) public data + CDC PLACES health outcomes  
**Language:** R  

---

## Business Problem

Environmental exposures — air pollution, water contamination, industrial emissions — are established risk factors for a range of chronic and acute health conditions. Quantifying the relationship between environmental exposure and health outcomes at a population level requires careful handling of geographic clustering, confounding by socioeconomic factors, and spatial autocorrelation. These methods are central to environmental epidemiology, environmental risk assessment, and regulatory impact analysis.

This project links publicly available air quality monitoring data to county-level health outcome estimates, building regression models that account for geographic structure and confounding — analogous to analyses conducted by environmental health researchers, insurers pricing climate-related health risk, and public health agencies evaluating pollution standards.

---

## Datasets

**Exposure:** EPA Air Quality System (AQS) — annual county-level PM2.5 (fine particulate matter) concentrations. Downloadable via the EPA AQS API or pre-aggregated annual summary files.

**Health outcomes:** CDC PLACES — county-level modelled estimates of chronic disease prevalence (asthma, COPD, cardiovascular disease, diabetes) for all US counties.

**Covariates:** US Census / American Community Survey — county-level poverty rate, median income, % uninsured, urban/rural classification.

**Sources:**
- [EPA AQS Annual Summary Data](https://aqs.epa.gov/aqsweb/airdata/download_files.html)
- [CDC PLACES](https://www.cdc.gov/places/)
- [US Census ACS 5-Year Estimates](https://data.census.gov/)

---

## Approach

### 1. Data ingestion and linkage
- Download EPA PM2.5 county annual summaries (2015–2020)
- Pull CDC PLACES outcome prevalence estimates by county
- Join Census socioeconomic covariates
- Final unit of analysis: US county × year (panel structure)

### 2. Exploratory spatial analysis
- Choropleth maps of PM2.5 exposure and asthma prevalence by county
- Scatter plots: exposure vs. outcome by region
- Correlation between PM2.5 and each health outcome

### 3. Confounder-adjusted regression
- OLS regression: PM2.5 predicting asthma prevalence, unadjusted
- Add socioeconomic confounders: poverty rate, % uninsured, urban/rural
- Assess residual confounding; discuss limitations of ecological inference

### 4. Accounting for geographic clustering
- Moran's I test for spatial autocorrelation in residuals
- Mixed model with state as random effect: county-level exposure within states
- Comparison of naive OLS vs. mixed model estimates and standard errors

### 5. Panel analysis (extension)
- Fixed-effects panel regression: within-county changes in PM2.5 predicting within-county changes in health outcomes over time
- Controls for all time-invariant county-level confounders

### 6. Results communication
- Side-by-side choropleth: PM2.5 and asthma prevalence
- Coefficient plot: unadjusted vs. adjusted associations
- Residual spatial autocorrelation map before and after mixed model
- Plain-language interpretation with explicit ecological fallacy caveat

---

## Methods

| Method | Purpose |
|---|---|
| Choropleth mapping | Spatial visualisation of exposure and outcome |
| OLS regression | Baseline association; unadjusted and adjusted |
| Mixed models (state random effect) | Account for geographic clustering within states |
| Moran's I | Test for spatial autocorrelation in residuals |
| Fixed-effects panel regression | Control for time-invariant county confounders |
| Confounder adjustment | Poverty, insurance, urban/rural as socioeconomic covariates |

---

## Key Packages

`tidyverse`, `sf`, `tigris`, `ggplot2`, `lme4`, `spdep`, `plm`, `broom`

---

## Business Framing

> *"Is county-level PM2.5 exposure associated with chronic respiratory disease prevalence after accounting for socioeconomic confounders and geographic clustering?"*

Results are framed around environmental risk assessment: adjusted associations with explicit uncertainty, spatial residual diagnostics that support (or challenge) the modelling assumptions, and a panel analysis that strengthens causal inference by exploiting within-county variation over time.

---

*industry_projects - Samantha McGarrigle*
