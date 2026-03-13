# User Retention and Funnel Analysis

**Domain:** Product Analytics  
**Dataset:** Online Retail II Dataset (UCI Machine Learning Repository)  
**Language:** R  

---

## Business Problem

Understanding how customers move through a purchase funnel, how many return after their first purchase, and when cohorts tend to disengage are foundational product analytics questions. Retention curves and cohort analysis directly inform product decisions — onboarding improvements, re-engagement campaigns, loyalty programmes — and are a standard component of growth analytics in any consumer-facing business. The same methodology applies to insurance renewal cycles, patient re-engagement in healthcare, and subscription retention in SaaS.

This project implements a full cohort retention and funnel analysis workflow using transactional retail data, producing the standard outputs a product analyst or growth team would present to stakeholders.

---

## Dataset

The Online Retail II dataset contains all transactions from a UK-based online retailer between 2009 and 2011. It is a real transactional dataset with sufficient complexity (cancellations, multi-item orders, international customers) to require genuine data cleaning before analysis.

**Key variables:**
- InvoiceNo: transaction identifier (prefix C = cancellation)
- StockCode, Description: product identifiers
- Quantity, UnitPrice: transaction details
- InvoiceDate: timestamp
- CustomerID: unique customer identifier
- Country: customer country

**Source:** [UCI Machine Learning Repository — Online Retail II](https://archive.ics.uci.edu/dataset/502/online+retail+ii)

---

## Approach

### 1. Data ingestion and cleaning
- Load dataset; inspect structure and missingness
- Remove cancelled transactions (InvoiceNo starting with C)
- Filter to customers with valid CustomerID
- Compute order-level revenue (Quantity × UnitPrice)
- Flag first purchase date per customer

### 2. Cohort construction
- Assign each customer to an acquisition cohort by month of first purchase
- Compute months since acquisition for each subsequent transaction
- Build cohort × period retention matrix

### 3. Retention analysis
- Cohort retention heatmap: % of each cohort still purchasing at each period
- Retention curves by cohort vintage
- Average retention curve across all cohorts
- Month-1 and Month-3 retention rates as headline KPIs

### 4. Revenue per cohort
- Average revenue per customer by cohort and period
- Cumulative revenue curves: lifetime value trajectory by cohort
- Identify highest-value cohort vintages

### 5. Funnel analysis
- Define purchase funnel stages: first visit (proxy: first transaction) → repeat purchase → 3+ purchases → 6+ purchases
- Compute conversion rates at each stage
- Drop-off analysis: where do customers disengage most?
- Logistic regression: which customer or product attributes predict conversion past stage 1?

### 6. RFM segmentation (extension)
- Recency, Frequency, Monetary segmentation
- K-means clustering on RFM scores
- Segment profiles: champions, at-risk, lost customers
- Retention rates by RFM segment

### 7. Results communication
- Cohort retention heatmap (standard product analytics deliverable)
- Retention curves with confidence bands
- Funnel bar chart with conversion rates at each stage
- RFM segment summary table

---

## Methods

| Method | Purpose |
|---|---|
| Cohort analysis | Retention over time by acquisition period |
| Retention matrix | Standard product analytics deliverable |
| Funnel analysis | Stage-by-stage conversion and drop-off |
| Logistic regression | Predictors of conversion past first purchase |
| K-means clustering | RFM-based customer segmentation |
| LTV curves | Cumulative revenue by cohort vintage |

---

## Key Packages

`tidyverse`, `lubridate`, `ggplot2`, `scales`, `tidymodels`, `cluster`, `factoextra`

---

## Business Framing

> *"What proportion of customers return after their first purchase, when do cohorts typically disengage, and what factors predict conversion to repeat buyers?"*

Results are presented as the standard product analytics deliverable: a cohort retention heatmap, headline KPIs (Month-1 and Month-3 retention), a funnel with stage-level conversion rates, and a segmentation that a marketing or product team could act on directly.

---

*industry_projects - Samantha McGarrigle*
