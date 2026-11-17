# Early ICU Risk Warning System

**Confidence-aware ICU risk prediction using multiple imputation and tree-based machine learning**

Developed as part of BMI5101 coursework at the National University of Singapore.

## Overview

ICU clinical data frequently contain missing measurements because variables are collected at different frequencies and according to individual patient needs. Missingness therefore presents an important challenge for predictive modeling: a single imputed value may conceal uncertainty about the patient's underlying clinical state.

This project developed an ICU risk-prediction workflow that propagates uncertainty from missing-data imputation into downstream patient-level predictions.

Rather than relying on a single completed dataset, multiple plausible datasets were generated using Multiple Imputation by Chained Equations (MICE). Risk predictions were then generated across the imputed datasets and pooled while retaining between-imputation variability.

The goal was not only to generate a final risk estimate, but also to identify predictions for which uncertainty from missing data could affect clinical risk classification.

## Research Question

**Can uncertainty arising from missing ICU measurements be propagated through a machine-learning pipeline to provide both patient-level risk estimates and information about prediction confidence?**

The project focused on three objectives:

- Develop an ICU risk-prediction workflow using multiply imputed clinical data.
- Quantify variation in predicted risk across imputations.
- Evaluate whether uncertainty-aware confidence information could help distinguish more reliable from less reliable risk classifications.

## Methodology

### 1. Clinical Data Preprocessing

The ICU dataset was prepared in Python through:

- Data cleaning and preprocessing
- Exploratory data analysis
- Assessment of missing-data patterns
- Preparation of clinical variables for downstream modeling

### 2. Multiple Imputation

Missing values were handled using **Multiple Imputation by Chained Equations (MICE)** implemented with `miceforest`.

The workflow generated:

- **5 imputed datasets**
- **5 MICE iterations**

This produced multiple plausible versions of the missing clinical measurements rather than replacing each missing value with a single estimate.

### 3. Risk Prediction

An **XGBoost** model was used to generate patient-level risk predictions for each imputed dataset.

The overall workflow was:

```text
Incomplete ICU data
        ↓
Multiple Imputation (MICE)
        ↓
5 imputed datasets
        ↓
XGBoost risk prediction
        ↓
5 imputation-specific risk estimates
        ↓
Pooling + uncertainty estimation
        ↓
Final risk estimate + confidence information
```

### 4. Prediction Pooling and Uncertainty

Risk predictions across the multiply imputed datasets were combined using a **Rubin-style multiple-imputation pooling framework**.

In addition to the pooled risk estimate, between-imputation variability was retained to characterize uncertainty associated with the missing-data imputation process.

This allowed uncertainty introduced during preprocessing to be propagated into downstream patient-level predictions rather than being discarded after imputation.

### 5. Confidence-Aware Risk Classification

A predefined **20% risk threshold** was used for risk classification.

Predictions were flagged as **LOW confidence** when their uncertainty interval crossed the 20% decision threshold.

For example:

```text
Pooled risk estimate = 21%

Uncertainty interval = 17%–25%

                    20% threshold
                         │
17% ─────────────────────┼──────── 25%

→ Risk classification depends on imputation uncertainty
→ LOW confidence
```

This confidence-gating approach was designed to highlight cases in which uncertainty associated with missing clinical measurements could affect the final risk classification.

### 6. Model Evaluation

Model performance was evaluated using:

- **5-fold cross-validation**
- **20 bootstrap resamples**
- **AUROC**
- **AUPRC**

The multiple-imputation workflow was also compared with a **single-imputation (SI)** workflow to assess whether multiple imputation improved predictive discrimination and provided additional uncertainty information.

## Results

### Predictive Performance

The multiple-imputation workflow achieved:

| Prediction Horizon | AUROC |
|---|---:|
| 6 hours | 0.62 |
| 12 hours | 0.72 |
| 24 hours | 0.68 |

Compared with single imputation, multiple imputation produced modest improvements in AUROC:

| Prediction Horizon | AUROC Improvement: MI vs SI |
|---|---:|
| 6 hours | +0.030 |
| 12 hours | +0.009 |
| 24 hours | +0.004 |

Bootstrap confidence intervals for these differences included zero, indicating that the observed improvements in discrimination were not statistically conclusive.

The primary contribution of multiple imputation in this project was therefore not simply improved model discrimination, but the ability to retain and propagate imputation-related uncertainty into patient-level predictions.

### Prediction Confidence

Uncertainty was greater at earlier prediction horizons:

- **6-hour predictions:** 23% were flagged as LOW confidence.
- **24-hour predictions:** 3% were flagged as LOW confidence.

Confidence-based gating improved discrimination at the **6-hour and 12-hour horizons**, while no improvement was observed at 24 hours.

These findings suggest that uncertainty information may be particularly relevant when predictions are made earlier and available clinical information is more incomplete.

## My Contributions

This was a team-based biomedical informatics project. My primary contributions focused on the **literature review and methodological development** of the project, including:

- Reviewed existing approaches to missing-data handling and multiple imputation in clinical risk-prediction studies.
- Examined methodological limitations associated with producing a single risk estimate after multiple imputation and contributed to defining the project's uncertainty-aware extension.
- Contributed to the design and discussion of the multiple-imputation, prediction-pooling, and confidence-assessment framework.
- Reviewed and interpreted the comparison between multiple- and single-imputation approaches and the implications of between-imputation uncertainty for patient-level risk prediction.
- Contributed to the interpretation and presentation of the final methodology and findings.

## Tech Stack

- **Python**
- **pandas**
- **miceforest**
- **XGBoost**
- **scikit-learn**
- Multiple Imputation by Chained Equations (MICE)
- Cross-validation
- Bootstrap evaluation
- Jupyter Notebook

## Key Takeaways

This project demonstrated an end-to-end clinical machine-learning workflow spanning:

**Clinical data preprocessing → missing-data analysis → multiple imputation → predictive modeling → uncertainty quantification → model evaluation**

A key methodological insight was that uncertainty introduced during missing-data handling can propagate into downstream predictions.

While multiple imputation did not produce conclusively superior discrimination compared with single imputation, it enabled the model to retain information about prediction uncertainty that would otherwise be lost when using a single completed dataset.

This distinction may be particularly useful for patient-level risk predictions close to a decision threshold.

## Project Context

- **Course:** BMI5101
- **Program:** MSc Biomedical Informatics (Analytics)
- **Institution:** National University of Singapore
- **Project Type:** Team-based coursework project
- **Status:** Completed, October 2026
