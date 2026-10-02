
## 1. Predicting Death After Myocardial Infarction

**Course:** BSTT 528, Machine Learning for Big Data in Health Analytics (Spring 2023)
**Report:** [`BSTT_528_Final.pdf`](BSTT_528_Final__Copy_.pdf) · **Code:** R (report appendix)

**Question.** Can routinely collected clinical data identify patients at high risk of dying from complications after a heart attack, and which factors drive that risk?

**Data.** The UCI Myocardial Infarction Complications dataset: 1,700 patients with 111 clinical and demographic features (history, ECG findings, labs, ICU medications) and 12 outcome variables. I combined the lethal outcome categories into a binary outcome (death from any complication, about 16% of patients).

**Approach.**
- Mean imputation for missing values
- Penalized regression (Ridge, Lasso, Elastic Net) with 10-fold cross-validation to select λ, plus sure independence screening (SIS) as a feature selection comparison
- Tree-based models: CART, random forest, and gradient boosting
- Model comparison by AUC; feature importance compared across models

**Results.**

| Model | AUC |
|---|---|
| **Elastic Net** | **0.838** |
| Lasso | 0.837 |
| CART | 0.816 |
| Ridge | 0.809 |
| Lasso with SIS | 0.79 |
| Random forest | ~0.73 |

Elastic Net performed best among the penalized models, though only marginally better than Lasso. Predictors that appeared consistently across models included chronic heart failure, right ventricular MI, relapse of pain during the hospital stay, systolic blood pressure in the ICU, and ICU use of opioids and NSAIDs.

**Looking back.**
- **Feature timing.** Several strong predictors (ICU medications on days 2–3, relapse of pain on day 3) are measured *after* admission. For a model meant to flag risk at admission, I'd restrict to features available at prediction time, which would likely lower AUC but make the model usable.
- **Boosting result.** The report shows boosting with AUC 0.887, but on review, the cross-validation loop overwrites its out-of-fold predictions and the model uses a Gaussian rather than Bernoulli loss. I don't treat that number as reliable, which is why Elastic Net is the headline here.
- **Leakage and reproducibility.** I'd impute inside each CV fold rather than on the full dataset, and set a random seed.
- **Beyond AUC.** For clinical use I'd also report calibration and sensitivity at a clinically meaningful threshold.

