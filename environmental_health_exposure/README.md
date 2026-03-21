# Environmental Health and Exposure Modelling

**Domain:** Public Health  
**Datasets:** EPA Air Quality System (AQS) · CDC PLACES · US Census ACS  
**Language:** R

---

## Business Problem

Environmental exposures — air pollution, water contamination, industrial emissions — are established risk factors for a range of chronic and acute health conditions. Quantifying the relationship between environmental exposure and health outcomes at a population level requires careful handling of geographic clustering, confounding by socioeconomic factors, and spatial autocorrelation. These methods are central to environmental epidemiology, environmental risk assessment, and regulatory impact analysis.

This project links publicly available air quality monitoring data to county-level health outcome estimates, building regression models that account for geographic structure and confounding — analogous to analyses conducted by environmental health researchers, insurers pricing climate-related health risk, and public health agencies evaluating pollution standards.

**Central question:** Is county-level PM2.5 exposure associated with chronic respiratory disease prevalence after accounting for socioeconomic confounders and geographic clustering?

---

## Datasets

**Exposure:** EPA Air Quality System (AQS) — annual county-level AQI summary files (2015–2020). The exposure variable is `Days.PM2.5`: the number of days per year on which PM2.5 was the dominant air pollutant in a given county. Downloaded as pre-aggregated annual CSV files from the EPA AQS data portal.

**Health outcomes:** CDC PLACES — county-level model-based estimates of chronic disease prevalence (asthma, COPD, cardiovascular disease, diabetes) for all US counties.

**Covariates:** US Census American Community Survey (ACS) 5-year estimates — county-level poverty rate, % uninsured, and median household income.

**Sources:**
- [EPA AQS Annual Summary Data](https://aqs.epa.gov/aqsweb/airdata/download_files.html)
- [CDC PLACES](https://www.cdc.gov/places/)
- [US Census ACS 5-Year Estimates](https://data.census.gov/)

See [`data/README.md`](data/README.md) for full dataset descriptions, file names, and column definitions.

---

## Approach

### 1. Data ingestion and linkage
- Load EPA AQS annual AQI county summary files (2015–2020)
- Assign FIPS codes by joining on state and county name via `tigris`
- Load CDC PLACES county health outcome prevalence estimates
- Load Census ACS socioeconomic covariates from local CSV files
- Join all sources on county FIPS code into a county × year panel

### 2. Exploratory spatial analysis
- Choropleth maps of PM2.5 exposure days and asthma prevalence by county
- Scatter plots: exposure vs. outcome by Census region
- Correlation matrix: PM2.5 days, all four health outcomes, and socioeconomic covariates

### 3. Confounder-adjusted regression
- OLS regression: PM2.5 days predicting asthma prevalence, unadjusted
- Sequential addition of socioeconomic confounders: poverty rate → % uninsured → log(median income)
- Coefficient plot across all four OLS specifications

### 4. Spatial autocorrelation diagnostics
- Moran's I test for spatial autocorrelation in fully adjusted OLS residuals
- Choropleth map of residuals showing geographic clustering pattern

### 5. Mixed model — state random effect
- `lme4` mixed model with state as random intercept
- Intraclass correlation coefficient (ICC) quantifying between-state variance
- Comparison of OLS vs. mixed model PM2.5 estimate and confidence interval

### 6. Panel fixed-effects regression
- `plm` one-way county fixed effects: within-county PM2.5 variation over 2015–2020
- `plm` two-way county + year fixed effects: additionally absorbs national trends
- Comparison of one-way vs. two-way FE estimates

### 7. Results summary
- Side-by-side choropleth: PM2.5 days and asthma prevalence
- Final coefficient plot across all 7 model specifications
- Plain-language interpretation and explicit ecological fallacy caveat

---

## Methods

| Method | Purpose |
|--------|---------|
| Choropleth mapping (`sf`, `tigris`) | Spatial visualisation of exposure and outcome |
| OLS regression | Baseline association; unadjusted and adjusted |
| Moran's I (`spdep`) | Test for spatial autocorrelation in residuals |
| Mixed model — state random effect (`lme4`) | Account for geographic clustering within states |
| County fixed-effects panel regression (`plm`) | Control for time-invariant county confounders |
| Two-way FE — county + year (`plm`) | Additionally absorb national year-to-year trends |

---

## Key Packages

```r
install.packages(c(
  "dplyr", "tidyr",
  "ggplot2", "sf", "tigris", "patchwork", "scales",
  "lme4", "broom", "broom.mixed",
  "spdep",
  "plm"
))
```

### Jupyter with R Kernel

```r
install.packages("IRkernel")
IRkernel::installspec()
```

```bash
jupyter notebook environmental_health_exposure.ipynb
```

---

## Repository Contents

```
.
├── README.md
├── environmental_health_exposure.ipynb    # Full analysis pipeline
└── data/
    └── README.md                          # Dataset descriptions and column definitions
```

---

## Limitations

**Ecological fallacy:** All models operate at the county level. County-level associations between PM2.5 exposure days and asthma prevalence do not describe individual-level causal relationships. Results should be interpreted as area-level associations.

**Exposure variable:** The EPA AQS AQI summary files report the number of days PM2.5 was the dominant pollutant — not a direct concentration measurement. This is a count-based proxy for PM2.5 burden rather than a continuous concentration in μg/m³.

**Cross-sectional outcomes:** CDC PLACES provides a single cross-section of health prevalence estimates. The panel FE model exploits within-county PM2.5 variation over time but cannot exploit within-county health outcome variation, limiting causal interpretation of the panel results.

**Monitor coverage:** EPA AQS monitors are not uniformly distributed. Rural and lower-pollution counties are underrepresented, producing non-random missingness that may introduce selection bias.

**Residual confounding:** Socioeconomic covariates do not capture all relevant confounders (e.g. smoking prevalence, occupational exposures, healthcare access beyond insurance status).

---

## Business Framing

> *"Is county-level PM2.5 exposure associated with chronic respiratory disease prevalence after accounting for socioeconomic confounders and geographic clustering?"*

Results are framed around environmental risk assessment: adjusted associations with explicit uncertainty, spatial residual diagnostics that support (or challenge) modelling assumptions, and a panel analysis that strengthens causal inference by exploiting within-county variation over time.

---

*industry_projects - Samantha McGarrigle*