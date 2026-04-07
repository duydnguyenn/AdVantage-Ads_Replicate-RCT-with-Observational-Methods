# AdVantage Digital: Replicating RCT Ground Truth with Observational Methods

**Author:** Duy Nguyen

**Dataset:** Facebook Ad Campaign (2,000,000 users)

---

## Overview

This project examines whether modern observational causal inference methods can replicate the results of a randomized controlled trial (RCT) in a digital advertising context. AdVantage Digital conducted a large-scale Facebook experiment using Ghost Ads methodology to establish an experimental ground truth (ATT). The treatment group is then used to evaluate three observational estimators — Naive Comparison, Adjusted Regression, Inverse Probability Weighting (IPW), and Double Machine Learning (Double ML) — by benchmarking their estimates against the RCT.

---

## Research Question

> *Can observational causal inference methods applied to treatment-group-only data reliably replicate the Average Treatment Effect on the Treated (ATT) obtained from a randomized controlled trial?*

---

## Dataset

The sample dataset (`data/AdVantage_sample.csv`) contains 50,000 observations drawn via stratified sampling from the full 2,000,000-user experiment, preserving the original 50/50 treatment/control split. The full dataset is not included in this repository due to file size constraints (243 MB).

| Variable | Type | Description |
|---|---|---|
| `age` | Integer (18–70) | User age in years |
| `gender` | Binary (0/1) | Gender indicator |
| `activity` | Continuous (0–1) | Platform engagement level |
| `num_friends` | Integer (100–1000) | Number of platform connections |
| `time_on_platform` | Continuous | Time spent on platform |
| `mobile_frac` | Continuous (0–1) | Proportion of mobile activity |
| `prior_conversion` | Continuous (0–1) | Historical conversion propensity |
| `income` | Continuous (25k–200k) | User income in USD |
| `urban_index` | Continuous (0–1) | Urbanization index |
| `Z` | Binary (0/1) | Treatment assignment (1 = treatment group) |
| `W` | Binary (0/1) | Ad exposure indicator (Ghost Ads for control) |
| `Y` | Binary (0/1) | Observed conversion outcome |

---

## Methods

1. **Randomization Check** — T-tests on key covariates to validate RCT balance
2. **Experimental ATT & ITT** — RCT-derived ground truth using Ghost Ads counterfactuals
3. **Naive Comparison** — Raw exposed vs. unexposed difference (unadjusted baseline)
4. **Adjusted Regression** — Logistic regression with full covariate adjustment
5. **Inverse Probability Weighting (IPW)** — Propensity score reweighting for covariate balance
6. **Double Machine Learning** — Orthogonalized ML-based doubly-robust estimator

---

## Key Findings

| Method | Absolute Lift | Direction of Bias |
|---|---|---|
| RCT ATT (Benchmark) | — | — |
| Naive Comparison | Overestimates | Upward (activity + targeting bias) |
| Adjusted Regression | Overestimates | Upward (residual confounding) |
| IPW | Closer to benchmark | Reduced, unobserved confounders remain |
| Double ML | Closest to benchmark | Near-unbiased under observable confounders |

Naive comparison substantially overstates advertising lift due to activity bias, targeting-induced endogeneity, and competition-induced endogeneity. Double ML achieves the best approximation to the experimental ground truth among observational methods.

---

## Repository Structure

```
AdVantage-Ads_Replicate-RCT-with-Observational-Methods/
├── AdVantage_Analysis.ipynb          # Main analysis notebook
├── [Case] AdVantage Digital.pdf      # Original case document
├── data/
│   └── AdVantage_sample.csv          # 50,000-row stratified sample
├── requirements.txt
└── .gitignore
```

---

## How to Run

1. Clone this repository:
   ```bash
   git clone https://github.com/duydnguyenn/AdVantage-Ads_Replicate-RCT-with-Observational-Methods.git
   cd AdVantage-Ads_Replicate-RCT-with-Observational-Methods
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

3. Launch Jupyter and open the notebook:
   ```bash
   jupyter notebook AdVantage_Analysis.ipynb
   ```

> **Note:** The Double ML cell (Question 6) uses Random Forest models and may take several minutes to complete.

---

## References

- Chernozhukov et al. (2018). Double/debiased machine learning for treatment and structural parameters. *The Econometrics Journal*, 21(1), C1–C68.
- Gordon, Moakler & Zettelmeyer (2023). Close enough? A large-scale exploration of non-experimental approaches to advertising measurement. *Marketing Science*, 42(4), 768–793.
- Case prepared by Prof. Amin Sayedi, Foster School of Business, University of Washington (2025).
