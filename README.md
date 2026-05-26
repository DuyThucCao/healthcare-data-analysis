# Healthcare Utilization & Billing Analysis

![R](https://img.shields.io/badge/R-4.3%2B-276DC3?logo=r&logoColor=white)
![tidyverse](https://img.shields.io/badge/tidyverse-2.0-1D7EC5?logo=r)
![tidymodels](https://img.shields.io/badge/tidymodels-1.1-orange)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)
![Status](https://img.shields.io/badge/status-active-brightgreen)

> An end-to-end R project covering data cleaning, exploratory analysis, interactive visualization, and a supervised machine learning pipeline to predict patient test outcomes in a simulated hospital dataset.

**[▶ View Live Interactive Report](https://duythuccao.github.io/healthcare-data-analysis/report.html)**

---

## Table of Contents

- [Overview](#overview)
- [Dataset](#dataset)
- [Key Findings](#key-findings)
- [Machine Learning Results](#machine-learning-results)
- [Methodology](#methodology)
- [Repository Structure](#repository-structure)
- [How to Reproduce](#how-to-reproduce)
- [Skills Demonstrated](#skills-demonstrated)
- [Contact](#contact)

---

## Overview

This project analyzes a synthetic healthcare dataset of **55,000+ patient encounters** across multiple conditions, insurance providers, and admission types. The analysis is structured in two parts:

1. **Exploratory Data Analysis (EDA)** — billing distributions, utilization patterns, demographic breakdowns, and group-level statistical tests.
2. **Machine Learning Pipeline** — a binary classification model predicting whether a patient will receive an *Abnormal* test result, using Logistic Regression and Random Forest with full cross-validation and model evaluation.

The workflow is designed to reflect realistic healthcare analytics practice: privacy-conscious data handling, reproducible code, and clear communication of findings for both technical and non-technical stakeholders.

---

## Dataset

| Field | Description |
|---|---|
| **Source** | Synthetic dataset (`healthcare_prediction_dataset.csv`) |
| **Records** | ~55,500 patient encounters |
| **Date range** | 2019–2024 |
| **Key variables** | Age, Gender, Blood Type, Medical Condition, Insurance Provider, Admission Type, Billing Amount, Length of Stay, Medication, Test Results |

> ⚠️ This is a **synthetic dataset** intended for portfolio and educational purposes. No real patient data is used.

The raw dataset contains inconsistent name casing and minor data quality issues, which are handled in the cleaning pipeline. Direct patient identifiers (name, doctor, hospital, room number) are excluded from all preview tables — even in synthetic data, modeling good practice matters.

---

## Key Findings

### Billing & Utilization

- **Median billing amount**: ~$25,000 per encounter, with a right-skewed distribution indicating a meaningful tail of high-cost cases.
- **Emergency admissions** drive higher median billing across nearly every condition, with the gap most pronounced for Diabetes and Cancer.
- **Length of stay** is positively associated with billing amount, but the relationship is weaker than expected — suggesting case complexity (captured by condition and admission type) matters more than raw days.
- **Weekday admissions** are nearly uniformly distributed, consistent with a general inpatient population rather than a purely elective-surgery facility.

### Conditions & Insurance

- **Arthritis, Cancer, and Obesity** have the highest median billing despite similar patient volumes to other conditions.
- **Medicare patients** skew older (as expected) and have a higher emergency admission share (~30%) compared to commercial payers.
- **Abnormal test results** are most concentrated in Diabetes and Hypertension encounters — a signal worth investigating for care pathway design.

### Statistical Tests

| Test | Result |
|---|---|
| Chi-square: Medical Condition × Admission Type | Significant (p < 0.001) — admission mix differs by condition |
| Pearson correlation: Age × Billing Amount | Weak positive (r ≈ 0.01, p < 0.05) |
| Multivariable regression R² | ~0.01 — condition and admission type are directional predictors, but billing variance is largely unexplained by available fields |

---

## Machine Learning Results

A binary classification task was defined: predict whether a patient's test result is **Abnormal** (1) vs. Normal or Inconclusive (0).

| Model | ROC-AUC (CV) | Accuracy | Precision | Recall | F1 |
|---|---|---|---|---|---|
| Logistic Regression (baseline) | ~0.50 | ~67% | — | — | — |
| Random Forest (tuned) | ~0.52 | ~67% | — | — | — |

> **Interpretation**: The low AUC is an honest and important finding. The available features (demographics, condition, insurance, admission type) have weak predictive signal for test outcomes in this synthetic dataset. This result demonstrates the ability to correctly evaluate a null or near-null finding — a critical skill in applied data science. Real-world improvement would require clinical variables (vitals, labs, procedure codes) not present here.

**Top predictors by Random Forest variable importance:**
1. Billing Amount
2. Length of Stay
3. Age
4. Medical Condition
5. Admission Type

---

## Methodology

```
Raw Data (55,500 records)
       │
       ▼
Data Cleaning & Validation
  • Standardize column names (janitor)
  • Parse and validate dates (lubridate)
  • Remove non-positive billing, invalid stays
  • Document exclusions in quality check table
       │
       ▼
Feature Engineering
  • Age groups (0-17, 18-34, 35-49, 50-64, 65+)
  • Length-of-stay bands (0-2, 3-7, 8-14, 15+ days)
  • Billing quartile tiers
  • Admission weekday
       │
       ▼
Exploratory Data Analysis
  • Summary tables by condition, insurer, admission type
  • Interactive plots (plotly): billing distribution,
    condition boxplots, age-billing scatter, LOS plots,
    weekday volume bar chart
  • Chi-square test, Pearson correlation
  • Multivariable linear regression for billing
       │
       ▼
Machine Learning Pipeline (tidymodels)
  • Binary target: Abnormal test result (yes/no)
  • 75/25 stratified train/test split
  • 5-fold cross-validation
  • Recipe: dummy encoding, normalization,
    zero-variance filter, SMOTE (class imbalance)
  • Models: Logistic Regression, Random Forest (ranger)
  • Evaluation: ROC-AUC, confusion matrix, F1
  • Feature importance: vip package
```

---

## Repository Structure

```
healthcare-data-analysis/
├── README.md                          ← You are here
├── LICENSE                            ← MIT
├── .gitignore                         ← R + DS standard ignores
│
├── data/
│   └── healthcare_prediction_dataset.csv
│
├── analysis/
│   ├── Healthcare-Data-Analysis.Rmd   ← EDA + statistical analysis
│   └── Healthcare-Data-Analysis-ML.Rmd ← Full ML pipeline
│
└── docs/
    ├── index.html                     ← GitHub Pages landing page
    └── report.html                    ← Rendered interactive report
```

---

## How to Reproduce

### Prerequisites

```r
install.packages(c(
  "tidyverse", "tidymodels", "janitor", "lubridate",
  "plotly", "DT", "knitr", "rmarkdown",
  "ranger", "themis", "vip", "yardstick", "scales"
))
```

### Run the analysis

```r
# Clone the repo
# git clone https://github.com/DuyThucCao/healthcare-data-analysis.git

# Open in RStudio, then knit either file:
rmarkdown::render("analysis/Healthcare-Data-Analysis.Rmd")
rmarkdown::render("analysis/Healthcare-Data-Analysis-ML.Rmd")
```

The rendered HTML reports will appear in your working directory. Move the output to `docs/` to update the GitHub Pages site.

### GitHub Pages setup

1. Push to GitHub
2. Go to **Settings → Pages**
3. Set source to **Deploy from a branch**, branch: `main`, folder: `/docs`
4. Your live report will be at `https://duythuccao.github.io/healthcare-data-analysis/`

---

## Skills Demonstrated

| Category | Tools / Concepts |
|---|---|
| **Data Wrangling** | `dplyr`, `tidyr`, `janitor`, `lubridate`, `forcats` |
| **Visualization** | `ggplot2`, `plotly` (interactive), `DT` (interactive tables) |
| **Statistical Analysis** | Chi-square test, Pearson correlation, multivariable linear regression |
| **Machine Learning** | `tidymodels`, logistic regression, random forest (`ranger`), cross-validation, ROC-AUC, SMOTE |
| **Reporting** | R Markdown, parameterized notebooks, `knitr`, HTML output |
| **Best Practices** | De-identification, data quality documentation, reproducible pipelines |
