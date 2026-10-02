# Predicting In-Hospital Mortality from the First 48 Hours of ICU Data

**Gradient-boosted trees ranked ICU patients by mortality risk reasonably well (ROC-AUC ≈ 0.84), but at the default threshold they caught only 1 in 10 deaths, and their 86.7% accuracy barely beat a model that predicts everyone survives (86.2%).**

This project uses the [PhysioNet/Computing in Cardiology Challenge 2012](https://physionet.org/content/challenge-2012/) data to compare eight classifiers for predicting in-hospital death, overall and by ICU type. It started as a final project for BSTT 550 (UIC, Spring 2024) and was revised in October 2026 to correct how the results were evaluated and reported.

📄 **[Read the full paper (PDF)](icu_mortality.pdf)**

![Model accuracy vs. no-skill baseline](figures/accuracy_vs_baseline.png)

## Key findings

- **Discrimination is moderate.** Gradient boosting reached a test-set ROC-AUC of about 0.84. Penalized logistic models (elastic net 0.838, lasso 0.837) were close behind.
- **Accuracy is misleading here.** With 14.2% mortality, predicting "survived" for everyone scores 0.862. Most models barely beat that, or fell below it.
- **Recall for deaths was 0.11** at the default 0.5 threshold: 35 of 330 deaths flagged in the test set.
- **Measured values beat measurement frequency.** Counts of how often each variable was recorded reached an AUC of only 0.78 with the same model.
- **Top predictors (SHAP):** age, Glasgow Coma Score late in the 48-hour window, urine output, weight, and blood pressure at admission.

## Data

- **Cohort:** 12,000 adult ICU stays of at least 48 hours, from MIMIC-II (challenge sets A, B, and C). After merging, 11,988 stays were usable.
- **Inputs:** 6 admission descriptors plus 37 time-stamped vitals and labs from the first 48 hours.
- **Outcome:** in-hospital death (14.2% of stays).

The data are not included in this repo. Download them from [PhysioNet](https://physionet.org/content/challenge-2012/).

## Approach

1. **Feature construction:** each variable was binned by hour since admission (00–47) and pivoted to one column per variable-hour pair, about 1,700 features in total. I built two feature sets: hourly mean values and hourly measurement counts.
2. **Cohorts:** the full cohort plus four ICU types (coronary care, cardiac surgery recovery, medical, surgical).
3. **Models:** logistic regression (plus lasso, ridge, elastic net), decision tree, random forest, gradient boosting, XGBoost, linear SVM, naive Bayes, and a neural network.
4. **Evaluation:** an 80/20 train/test split per cohort, scored on accuracy, precision, recall, F1, ROC-AUC, and the confusion matrix.
5. **Interpretation:** SHAP values and impurity-based feature importance.

## What I'd do differently

The paper's Discussion lists the limitations in full. The most important fixes:

- **Metrics:** lead with ROC-AUC, PR-AUC, and calibration instead of accuracy.
- **Threshold:** use class weighting and choose the threshold on validation data.
- **Missing values:** replace zero-filling with per-variable summaries and missingness indicators.
- **Tuning:** tune hyperparameters with cross-validation only, never on the test set.
- **Benchmark and uncertainty:** compare against the SAPS-I score and report bootstrap confidence intervals.

## Repository contents

| File | Description |
| --- | --- |
| `icu_mortality.pdf` | Full paper |
| `icu_mortality.tex` | LaTeX source |
| `figures/` | All figures used in the paper |

## Tools

Python (pandas, scikit-learn, XGBoost, SHAP, matplotlib, seaborn) and LaTeX.

## Reference

Silva I, Moody G, Scott DJ, Celi LA, Mark RG. Predicting in-hospital mortality of ICU patients: The PhysioNet/Computing in Cardiology Challenge 2012. *Computing in Cardiology*. 2012;39:245–248.

---

**Michelle Carrizosa, M.S.** · M.S. Biostatistics, University of Illinois Chicago
