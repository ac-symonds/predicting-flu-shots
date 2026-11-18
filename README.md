# predicting-flu-shots

# Flu Shot Predictability

Anna Caroline Symonds and Margaret Lambdin, COSC 74

## Goal

Train machine learning models to:
- Determine whether data reported in the CDC's 2025 Behavioral Risk Factor Surveillance System (BRFSS) can be used to identify individuals and communities at risk of not receiving an annual influenza vaccination.
- Identify which characteristics are the strongest predictors of vaccination.

## Description

This project uses the CDC's 2025 BRFSS survey to predict whether a respondent reports having received a flu shot (`FLUSHOT7`) from demographic, access-to-care, and health variables.

**Data preparation**
- The original file has 356,158 rows and 283 columns.
- The flu-shot label was recoded to binary (1 = vaccinated, 0 = unvaccinated). Respondents who answered "don't know" or "refused," or who were not asked, were dropped, leaving 328,003 rows.
- Eight predictors were selected: age group (`_AGEG5YR`), sex (`_SEX`), race/ethnicity (`_RACE`), income (`_INCOMG1`), education (`_EDUCAG`), primary insurance (`PRIMINS2`), general health (`GENHLTH`), and state (`_STATE`).
- Rows missing any predictor (most often income) were dropped, leaving **262,645** rows.
- Two feature matrices were built: one-hot encoding of the nominal variables (state, race, insurance, sex) for the linear models, and integer codes for the tree-based models. Ordered variables (income, education, age, general health) were treated as numeric.
- The data was split 80/20 into training (210,116 rows) and test (52,529 rows) sets, stratified by label, with random seed 0.

**Models**
- Five models were trained: logistic regression with L1 regularization, logistic regression with L2 regularization, a linear support vector machine (SVM), a decision tree, and a random forest.
- Hyperparameters were tuned with 3-fold cross-validation on a 60,000-row subsample of the training set, scored by ROC-AUC.
- Performance was compared against a majority-class baseline on the held-out test set using accuracy, ROC-AUC, precision, recall, and F1 for the unvaccinated class, and log loss.
- Further analysis includes a feature-order analysis, permutation importance, and subgroup performance.

## Requirements

Python 3 (developed in Google Colab).

Packages: `autograd`, `numpy`, `pandas`, `matplotlib`, `scikit-learn`, `joblib`

The notebook mounts Google Drive through `google.colab`, so it is intended to be run in Colab.

## Data

The 2025 BRFSS data is not included in this repository because of its size. Download it from the CDC:
- Annual data page: https://www.cdc.gov/brfss/annual_data/annual_2025.html
- Direct download: https://www.cdc.gov/brfss/annual_data/2025/files/LLCP2025XPT.zip

Citation: Centers for Disease Control and Prevention. (2026). *Behavioral Risk Factor Surveillance System survey data, 2025* [SAS Transport Format]. U.S. Department of Health and
