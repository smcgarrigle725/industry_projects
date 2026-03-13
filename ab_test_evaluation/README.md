# A/B Test Design and Evaluation

**Domain:** Product Analytics  
**Dataset:** Udacity A/B Testing Final Project Dataset (public)  
**Language:** R  

---

## Business Problem

A/B testing is the primary tool for evidence-based product decisions. Whether evaluating a new onboarding flow, a pricing change, or a UI redesign, a well-designed and correctly analysed experiment is what separates a data-driven decision from a guess. Poorly designed or analysed experiments are pervasive — underpowered tests, multiple testing without correction, peeking at results early, and ignoring heterogeneous treatment effects are common failure modes that a skilled analyst must identify and avoid.

This project implements a complete A/B test analysis workflow: pre-experiment power analysis, sanity checks on randomisation, primary metric evaluation using both frequentist and Bayesian approaches, subgroup analysis, and novelty effect detection. The Udacity dataset provides a realistic experimental context with realistic complications.

---

## Dataset

The Udacity A/B Testing dataset is from a real experiment run by Udacity on their course enrollment flow. The experiment tested whether adding a screener question (asking students about their time commitment) before enrollment would reduce early course cancellations without significantly reducing enrollments.

**Experimental design:**
- Unit of randomisation: cookie (website visit)
- Control: standard enrollment page
- Treatment: enrollment page with time commitment screener question
- Primary metric: net conversion (payments / unique cookies)
- Secondary metric: gross conversion (enrollments / unique cookies)
- Invariant metric: number of cookies (sanity check)

**Source:** [Udacity A/B Testing Course — Final Project Data](https://www.kaggle.com/datasets/mariapushkareva/udacity-ab-testing-final-project)

---

## Approach

### 1. Experiment design review
- Document the experimental hypothesis, unit of randomisation, and metric definitions
- Distinguish between invariant metrics (sanity checks) and evaluation metrics
- Power analysis: minimum detectable effect, required sample size, expected duration

### 2. Sanity checks
- Verify that invariant metrics (cookie counts) are balanced between control and treatment
- Chi-square test for randomisation balance
- Day-of-week and time trend checks to rule out temporal confounding

### 3. Primary metric analysis: frequentist
- Two-proportion z-test for gross conversion and net conversion
- Effect size with 95% confidence interval
- Statistical significance and practical significance (minimum detectable effect threshold)
- Bonferroni correction for multiple metrics

### 4. Primary metric analysis: Bayesian
- Beta-binomial model for conversion rates
- Posterior distributions for control and treatment conversion rates
- Posterior probability that treatment > control
- Credible intervals; comparison with frequentist CIs

### 5. Sequential testing and peeking
- Demonstrate the peeking problem: show how p-values evolve if checked daily
- Alpha-spending approach: O'Brien-Fleming boundaries
- Compare sequential vs. fixed-horizon conclusions

### 6. Subgroup analysis
- Segment by day of week, new vs. returning visitors (if available in data)
- Interaction test: does the treatment effect differ by subgroup?
- Explicit correction for multiple subgroup comparisons

### 7. Novelty effect assessment
- Plot treatment effect over time: does it attenuate in later days?
- Discussion: how long should an experiment run to distinguish novelty from sustained effect?

### 8. Results communication
- Power analysis summary with sample size curve
- Sanity check table
- Effect size plot with CI for each metric
- Posterior distribution plot (Bayesian)
- Sequential test chart
- Go / No-go recommendation with explicit reasoning

---

## Methods

| Method | Purpose |
|---|---|
| Power analysis | Pre-experiment sample size and MDE calculation |
| Two-proportion z-test | Frequentist primary metric evaluation |
| Beta-binomial Bayesian model | Posterior probability of treatment superiority |
| Bonferroni correction | Multiple metric adjustment |
| O'Brien-Fleming boundaries | Sequential testing / alpha spending |
| Interaction tests | Subgroup heterogeneity of treatment effect |
| Novelty effect analysis | Temporal pattern of treatment effect |

---

## Key Packages

`pwr`, `BayesFactor`, `ggplot2`, `tidyverse`, `gsDesign`, `broom`

---

## Business Framing

> *"Did the screener question meaningfully reduce early cancellations, and are we confident enough in the result to launch the change to all users?"*

Results are presented as a complete experiment report: sanity checks, primary metric evaluation with both frequentist and Bayesian conclusions, subgroup results, and an explicit launch recommendation with the reasoning documented — the standard deliverable from an experimentation platform or growth analytics team.

---

*industry_projects - Samantha McGarrigle*
