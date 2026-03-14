# Infectious Disease Surveillance and Intervention Analysis

**Domain:** Public Health  
**Dataset:** CDC FluView influenza surveillance data (public API)  
**Language:** Python  

---

## Business Problem

Disease surveillance systems monitor population-level health trends over time, detecting seasonal patterns, outbreak signals, and the effects of public health interventions. Analysing surveillance time series requires methods that handle strong seasonality, irregular reporting, and the evaluation of policy changes (vaccination campaigns, school closures, travel restrictions) as natural experiments. These skills are directly transferable to pharmacovigilance, health economics, and any setting where temporal health data is routinely collected.

This project applies a full surveillance analysis workflow to publicly available influenza data, from seasonal decomposition through interrupted time series analysis of an intervention period.

---

## Dataset

CDC FluView provides weekly influenza surveillance data collected through the US Outpatient Influenza-like Illness Surveillance Network (ILINet) and virological testing. Data is accessed via direct API download.

**Key variables:**
- Week ending date
- % of visits for influenza-like illness (ILI%) by region
- Virological: % positive influenza A and B tests, total specimens
- Baseline and epidemic threshold by region and season

**Period:** 2010–2023 (national and HHS region level)  
**Source:** [CDC FluView Interactive](https://gis.cdc.gov/grasp/fluview/fluportaldashboard.html)

---

## Approach

### 1. Data ingestion
- Download national and regional ILI% data via CDC FluView API or direct CSV download
- Parse and construct weekly `pandas` time series with `DatetimeIndex`
- Align flu seasons (week 40 to week 39 of following year)

### 2. Exploratory time series analysis
- Multi-season overlay plot: visualise consistent seasonal pattern
- Epidemic threshold exceedance by season
- Comparison of 2009 H1N1 pandemic season vs. typical seasons
- 2020–21 season: near-elimination of flu during COVID-19 NPI period

### 3. Seasonal decomposition
- STL decomposition via `statsmodels.tsa.seasonal.STL`
- Seasonal strength and trend strength metrics
- Anomaly detection on remainder component

### 4. SARIMA modelling
- Stationarity testing via `statsmodels` ADF and KPSS tests
- Seasonal differencing and model identification via ACF/PACF
- SARIMA fitting via `statsmodels.tsa.statespace.SARIMAX`
- Residual diagnostics; in-sample fit and short-term forecast

### 5. Interrupted time series analysis
- Intervention: COVID-19 non-pharmaceutical interventions (March 2020)
- Segment the series pre- and post-intervention
- SARIMAX with intervention indicator: estimate immediate level change and slope change
- Counterfactual: what would ILI% have been without NPIs?
- Visualise observed vs. counterfactual with confidence intervals

### 6. Regional comparison
- Repeat key analyses across HHS regions using a loop over region subsets
- Summary of regional variation in seasonal peak timing and magnitude

### 7. Results communication
- Multi-panel seasonal overlay plot
- STL decomposition figure
- Interrupted time series plot with counterfactual
- Regional peak timing summary table

---

## Methods

| Method | Purpose |
|---|---|
| STL decomposition | Seasonal + trend extraction from weekly surveillance data |
| SARIMA | Seasonal ARIMA for weekly flu time series |
| Interrupted time series | Causal inference for the effect of NPIs on ILI% |
| SARIMAX with intervention terms | Estimate level and slope change at intervention point |
| Counterfactual forecasting | Estimate what would have happened without intervention |
| Regional comparison | Variation in seasonal pattern across HHS regions |

---

## Key Packages

`pandas`, `numpy`, `statsmodels`, `matplotlib`, `seaborn`, `requests`

---

## Business Framing

> *"How did non-pharmaceutical interventions during the COVID-19 pandemic affect influenza circulation, and what does the counterfactual suggest about the magnitude of that effect?"*

Results are framed around public health decision-making: seasonal baselines, epidemic threshold exceedance, and a quantified estimate of the intervention effect with uncertainty bounds — the kind of analysis that would appear in an epidemiological bulletin or health technology assessment.

---

*industry_projects - Samantha McGarrigle*
