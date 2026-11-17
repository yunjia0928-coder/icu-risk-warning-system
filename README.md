# Early ICU Risk Warning System

Machine-learning workflow for ICU risk prediction under missing clinical data, developed as part of BMI5101 coursework at the National University of Singapore.

## Overview

ICU datasets frequently contain missing measurements because clinical variables are collected at different frequencies and according to patient needs. Missingness therefore becomes an important consideration when developing predictive models from longitudinal clinical data.

This project explored a risk-prediction workflow that combines multiple imputation with tree-based machine learning. Instead of relying on a single imputed dataset, multiple plausible versions of the missing data were generated and propagated through the prediction workflow to examine variation in predicted risk.

## Research Question

Can multiple imputation be incorporated into an ICU risk-prediction workflow to account for uncertainty arising from missing clinical measurements?

The project focused on two related objectives:

- Develop a predictive modeling workflow for ICU risk using multiply imputed clinical data.
- Examine variation in predicted risk across imputations as an additional indicator of prediction uncertainty.

## Workflow

### 1. Data preprocessing

- Cleaned and prepared ICU clinical data in Python.
- Conducted exploratory data analysis.
- Assessed patterns and extent of missing data.
- Prepared variables for downstream modeling.

### 2. Multiple imputation

Missing clinical values were handled using Multiple Imputation by Chained Equations (MICE), generating multiple plausible completed datasets rather than relying on a single imputed value.

### 3. Risk prediction

An XGBoost model was applied to the imputed datasets to generate patient-level risk predictions.

### 4. Prediction pooling and uncertainty

Predictions generated across multiple imputations were combined to obtain an overall risk estimate.

Variation across the imputation-specific predictions was also examined to characterize uncertainty associated with the missing-data imputation process.

### 5. Model evaluation

Model performance was assessed using:

- Cross-validation
- Bootstrap-based evaluation
- Predictive performance metrics used in the final analysis

## Tech Stack

- Python
- pandas
- scikit-learn
- XGBoost
- Multiple imputation / MICE
- Jupyter Notebook

## My Contributions

This was a team-based biomedical informatics project.

My contributions included:

- Clinical data cleaning and preprocessing
- Exploratory data analysis
- Missing-data assessment
- Multiple-imputation workflow

## Key Takeaways

This project provided hands-on experience with an end-to-end clinical machine-learning workflow, including data preprocessing, missing-data handling, predictive modeling, and model evaluation.

It also highlighted an important modeling consideration: uncertainty introduced during preprocessing can propagate into downstream predictions and should be considered when interpreting patient-level risk estimates.

## Project Context

Course: BMI5101  
National University of Singapore  
MSc Biomedical Informatics (Analytics)

Completed: October 2026
