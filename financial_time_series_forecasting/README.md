# Financial Time Series Forecasting

**Domain:** Finance  
**Dataset:** Public commodity and index price data via `tidyquant` / `quantmod`  
**Language:** R  

---

## Business Problem

Accurate forecasting of commodity prices and financial indices is a fundamental problem in quantitative finance, risk management, and supply chain planning. Energy companies hedge fuel costs, insurers model asset returns, and portfolio managers forecast benchmark indices — all requiring rigorous time series methodology with honest uncertainty quantification.

This project applies a structured forecasting workflow to publicly available financial time series, comparing multiple model families, evaluating forecast accuracy via rolling-origin cross-validation, and communicating uncertainty through prediction intervals. The emphasis is on rigorous methodology and honest evaluation rather than claim of predictive superiority.

---

## Dataset

Price data downloaded directly in R using `tidyquant` and `quantmod` from Yahoo Finance and FRED (Federal Reserve Economic Data). No manual downloads required.

**Series used:**
- West Texas Intermediate (WTI) crude oil prices (FRED: DCOILWTICO)
- S&P 500 index (Yahoo Finance: ^GSPC)
- Gold spot price (Yahoo Finance: GC=F)

**Frequency:** Daily, aggregated to weekly for modelling  
**Period:** Approximately 2010–2023

---

## Approach

### 1. Data ingestion
- Pull series directly via `tidyquant::tq_get()` and `quantmod::getSymbols()`
- Aggregate daily to weekly closing prices
- Log-transform prices; work with log-returns where appropriate

### 2. Exploratory time series analysis
- Time series plots with trend and key events annotated
- Stationarity testing: ADF and KPSS tests
- ACF/PACF for transformed series
- Volatility clustering assessment (relevant for financial series)

### 3. Model fitting and comparison
- **ARIMA:** manual identification via ACF/PACF + auto.arima() comparison
- **ETS (Exponential Smoothing):** state space models via `fable`; automatic model selection
- **Prophet:** trend changepoint detection, weekly/annual seasonality, holiday effects
- **Seasonal naive:** benchmark model for comparison

### 4. Forecast evaluation: rolling-origin cross-validation
- Rolling-origin CV with `rsample::sliding_period()`
- Forecast horizons: 4, 8, and 12 weeks ahead
- Metrics: RMSE, MAE, MASE, coverage of prediction intervals
- Model comparison table and visualisation

### 5. Final forecasts and uncertainty
- Best model selected on CV performance
- 12-week ahead forecast with 80% and 95% prediction intervals
- Residual diagnostics on final model

### 6. Results communication
- Forecast plot with prediction intervals and historical context
- Model comparison table (all metrics, all horizons)
- Decomposition plot showing trend and seasonal components
- Plain-language interpretation of forecast uncertainty

---

## Methods

| Method | Purpose |
|---|---|
| ARIMA | Classical parametric time series model |
| ETS / state space models | Exponential smoothing with automatic structure selection |
| Prophet | Flexible trend + seasonality; handles irregular data well |
| Rolling-origin CV | Honest forecast evaluation respecting temporal order |
| MASE | Scale-independent accuracy metric; benchmark-relative |
| Prediction intervals | Uncertainty quantification for operational use |

---

## Key Packages

`tidyquant`, `quantmod`, `fable`, `fabletools`, `feasts`, `prophet`, `rsample`, `ggplot2`

---

## Business Framing

> *"What is our best estimate of WTI crude oil prices over the next 12 weeks, and how wide is the uncertainty around that estimate?"*

Results are framed around risk management use cases: forecast point estimates alongside explicit uncertainty bounds, a model performance comparison that demonstrates methodological rigour, and honest discussion of the limits of financial forecasting.

---

*industry_projects - Samantha McGarrigle*
