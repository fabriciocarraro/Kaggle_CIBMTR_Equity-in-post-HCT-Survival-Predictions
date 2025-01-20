# Kaggle Challenge - Improving Survival Predictions for Allogeneic HCT

https://www.kaggle.com/competitions/equity-post-HCT-survival-predictions/overview

This repository contains code and documentation for tackling a Kaggle-style survival analysis challenge, aiming to improve predictions of event‐free survival for patients undergoing allogeneic Hematopoietic Cell Transplantation (HCT). The solution focuses on building a Cox Proportional Hazards model while carefully preprocessing the data and accounting for fairness across multiple patient subgroups.

-----

## Competition Overview

From the Kaggle challenge description:

### Problem Statement
Improving survival predictions for allogeneic HCT patients is a critical healthcare challenge. Current models do not sufficiently capture disparities related to socioeconomic status, race, and geography. This competition encourages building models that can accurately and fairly predict survival rates across diverse racial groups.

### Synthetic Dataset
The competition provides a synthetically generated dataset, using the SurvivalGAN approach from the synthcity library. The data preserves realistic distributions and censoring patterns but does not expose private patient information.

------

## Goal and Evaluation

The goal is to develop predictive models that are accurate and fair. Fairness is addressed by evaluating performance (via the concordance index) within each racial group and then combining these group‐wise scores in a way that penalizes disparities.

### Stratified Concordance Index

- **Concordance Index (C‐index)** measures a survival model’s ability to rank patients correctly by their survival times. A higher C‐index means better discrimination, where:
  - 0.5 is the expected result from random predictions
  - 1.0 is a perfect model
  - 0.0 is perfectly wrong (anti-concordance)
 
- **Stratified C‐index**: The competition modifies the usual C‐index by computing it separately for each racial group, then combining those values as `mean(c‐index_across_groups) - std(c‐index_across_groups)`. This encourages consistently good performance across all groups rather than optimizing for just the majority population.

-----

## Dataset Description

- Training Data (`train.csv`): Contains 59 features plus 2 key survival columns:
  - `efs` — Event-free survival (0/1 event occurrence)
  - `efs_time` — Time to the event or censoring
- Test Data (`test.csv`): Contains the same features (minus `efs` and `efs_time`), used for generating final risk scores.
- Sample Submission (`sample_submission.csv`): Illustrates the expected output format (ID, prediction).
- Data Dictionary (`data_dictionary.csv`): Explains each feature.

The data includes demographic and clinical variables about both recipients and donors, such as:
- Recipient and donor ages, races, and sexes
- Disease status
- Genetic matching (HLA and more)
- Karnofsky performance scores
- Comorbidity indices

All data was generated synthetically to emulate real-world scenarios while preserving privacy.

-----

## Dependencies

Install them (if not already) via:

`pip install pandas numpy matplotlib scikit-learn plotly lifelines nbformat`

-----

## Model Training

The solution uses the Cox Proportional Hazards model from lifelines:

1. K-Fold Cross-Validation
  - Different penalizer values ([0.001, 0.01, 0.1, 1.0]) are tested, and the one yielding the highest mean C‐index is selected.
2. Model Fitting
  - The best Cox model is trained on the entire training dataset using the chosen penalizer.

-----

If you have any questions, suggestions, or issues, please open an issue or submit a pull request. Good luck improving event-free survival predictions for allogeneic HCT patients!
