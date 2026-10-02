# 2. Cervical Cancer Risk Factors

**Course:** BSTT 527 (Fall 2022)
**Report:** [`BSTT_FINAL_TEXT___527.pdf`](BSTT_FINAL_TEXT___527.pdf) · **Code:** R

**Question.** Almost all cervical cancer is linked to HPV, and HPV vaccine hesitancy is rising. What other factors are associated with a positive biopsy?

**Data.** The UCI Cervical Cancer (Risk Factors) dataset: 858 patients from Hospital Universitario de Caracas, with demographics, sexual and reproductive history, STD history, and screening test results. Only 54 patients (about 6%) had a positive biopsy, making this a rare-event problem.

**Approach.**
- Scaled continuous variables and checked correlations
- Used a decision tree for exploratory feature selection
- Fit logistic regression, Naive Bayes, and gradient boosting (GBM) on the full and reduced feature sets
- Evaluated with confusion matrices, focusing on sensitivity given the rare outcome

**Results.**
- The Schiller iodine test was the strongest predictor, followed by age and years of hormonal contraceptive use. Smoking history, age at first intercourse, and number of partners also contributed.
- The decision tree produced interpretable risk paths, such as a negative Schiller test combined with more than 8.5 years of hormonal contraceptive use.
- Naive Bayes performed best on this data. Adding the next four GBM-selected features changed accuracy by less than 1 point.

**Looking back.**
- **Imbalance.** With a 6% positive rate, a model that predicts "negative" for everyone is about 94% accurate, so accuracy says little. I'd lead with sensitivity, precision, and PR-AUC.
- **Validation.** A 90/10 split leaves only about 5 positive cases in the test set, so results are unstable. I'd use stratified repeated cross-validation instead.
- **Target-adjacent features.** Schiller, Hinselmann, and cytology are diagnostic tests closely tied to the biopsy outcome. To answer the actual question (risk factors *beyond* screening), I'd fit a second model that excludes them.
