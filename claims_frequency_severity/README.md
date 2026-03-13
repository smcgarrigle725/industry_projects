# Insurance Claims Frequency and Severity Modelling

**Domain:** Insurance  
**Dataset:** French Motor Third-Party Liability (MTPL) Insurance Dataset (`CASdatasets`)  
**Language:** R  

---

## Business Problem

Insurance pricing depends on two separate but related predictions: how often a policyholder will file a claim (frequency) and how large that claim will be when it occurs (severity). Multiplying the expected frequency by the expected severity gives the **pure premium** — the actuarially fair price before expenses and profit loading. This two-part modelling approach is standard practice in property and casualty insurance and forms the foundation of generalised linear model (GLM) pricing frameworks used across the industry.

This project implements the full frequency-severity workflow on a real motor insurance dataset, producing a pure premium model with interpretable rating factors — directly analogous to what an actuarial data scientist would build in a commercial lines pricing team.

---

## Dataset

The French Motor Third-Party Liability dataset from the `CASdatasets` R package contains policy-level records from a French motor insurer. It is one of the most commonly used public datasets in actuarial science.

**Key variables:**
- Policy: exposure (years), vehicle age, vehicle power, vehicle brand, vehicle gas type
- Policyholder: driver age, driver licence age, region, area (urban/rural density)
- Outcome (frequency): claim count per policy-year
- Outcome (severity): average claim amount per claim (separate dataset: `freMTPLsev`)

**Source:** `CASdatasets` R package — `freMTPL2freq` and `freMTPL2sev`

---

## Approach

### 1. Data ingestion and preparation
- Load `freMTPL2freq` (frequency) and `freMTPL2sev` (severity) from `CASdatasets`
- Join datasets; compute exposure-adjusted claim rates
- Inspect distribution of claim counts (zero-inflation) and claim amounts (right-skew)

### 2. Exploratory data analysis
- Claim frequency and severity by key rating factors
- Exposure-weighted summaries by vehicle power, driver age, and region
- Distribution plots: claim count histogram, severity density

### 3. Frequency model
- Poisson GLM with log(exposure) offset: baseline frequency model
- Check for overdispersion; fit negative binomial if warranted
- Rating factor interpretation: multiplicative rate relativities
- Residual diagnostics via `DHARMa`

### 4. Severity model
- Gamma GLM with log link for claim amount (conditional on claim occurring)
- Alternative: Tweedie GLM as a single-model combined approach
- Comparison of Gamma two-part vs. Tweedie one-part model

### 5. Pure premium
- Pure premium = predicted frequency × predicted severity
- Rate relativity tables by key rating factors
- Visualisation of pure premium surface across driver age and vehicle power

### 6. Model validation
- Cross-validation of frequency and severity models separately
- Lorenz curve and Gini index for model lift assessment (actuarial standard)
- Double lift chart: comparison of predicted vs. observed pure premium by decile

### 7. Results communication
- Rating factor tables (relativities with confidence intervals)
- Pure premium heatmap
- Lorenz curve / Gini
- Plain-language business summary

---

## Methods

| Method | Purpose |
|---|---|
| Poisson GLM with offset | Claim frequency model; exposure-adjusted rate |
| Negative binomial GLM | Overdispersed frequency alternative |
| Gamma GLM | Claim severity model; positive right-skewed amounts |
| Tweedie GLM | Combined frequency-severity single model |
| DHARMa diagnostics | Residual checking for count and continuous GLMs |
| Lorenz curve / Gini | Actuarial model lift assessment |
| Rate relativities | Multiplicative pricing factors by rating variable level |

---

## Key Packages

`CASdatasets`, `tidyverse`, `DHARMa`, `ggplot2`, `broom`, `MASS`

---

## Business Framing

> *"What is the actuarially fair pure premium for each policyholder, and which rating factors have the largest effect on expected claims cost?"*

Results are presented as a rate relativity table — the standard actuarial deliverable — alongside model diagnostics and a pure premium surface that a pricing team could use to review adequacy by segment.

---

*industry_projects - Samantha McGarrigle*
