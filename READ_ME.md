# Biostatistics M.S. Projects

Selected projects from my M.S. in Biostatistics (Big Data Health Analytics concentration) at the University of Illinois Chicago, completed alongside my work as a data scientist. They cover clinical risk prediction on high-dimensional and imbalanced data, and NLP on large-scale public text.

Each project ends with a **Looking back** section: what I'd change now with more experience. I think that's as useful as the results.

| Project | Question | Data | Methods | Headline result |
|---|---|---|---|---|
| [MI Mortality Prediction](#1-predicting-death-after-myocardial-infarction) | Which patients are at highest risk of death after a heart attack? | 1,700 patients, 111 clinical features | Ridge, Lasso, Elastic Net, CART, random forest, boosting (R) | Elastic Net AUC 0.84 (10-fold CV) |
| [Cervical Cancer Risk Factors](#2-cervical-cancer-risk-factors) | Beyond HPV, what predicts a positive biopsy? | 858 patients, 54 positive biopsies | Decision tree, logistic regression, Naive Bayes, GBM (R) | Schiller test, age, and years of hormonal contraception were the strongest predictors |
| [Public Discussion After Roe v. Wade](#3-public-discussion-after-roe-v-wade) | What themes and sentiment appeared on Twitter after the decision? | 415K tweets | TF-IDF, k-means, LDA topic modeling, VADER sentiment (Python) | Four main themes; lexicon sentiment conflicted with topic content |

---

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

---

## 2. Cervical Cancer Risk Factors

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

---

## 3. Public Discussion After Roe v. Wade

**Course:** BSTT 426, Machine Learning Using Python
**Report:** [`BSTT_426_FINAL_TEXT.pdf`](BSTT_426_FINAL_TEXT.pdf) · **Code:** Python

**Question.** What themes did people discuss on Twitter after Roe v. Wade was overturned in June 2022, and what was the sentiment of that discussion?

**Data.** 415,372 tweets from June to August 2022 (Kaggle), including tweet text, engagement, and account metadata. The analysis focused on tweet text.

**Approach.**
- Text preprocessing with NLTK: lowercasing, removing URLs/HTML/punctuation, stopword removal, stemming, lemmatization
- Bag-of-words and TF-IDF representations
- K-means clustering with the elbow method to estimate the number of themes (2–5)
- LDA (Latent Dirichlet Allocation) topic modeling with gensim, using 4 topics
- VADER sentiment analysis

**Results.**
- Four themes emerged: voting and choice, women's rights, the legal and state-level implications of the ruling, and abortion as healthcare.
- VADER classified more tweets as positive than negative, which conflicted with the content of the topics, most of which expressed concern about the ruling's implications.

**Looking back.**
- **Lexicon sentiment struggles with political text.** VADER misses sarcasm, negation, and stance (a tweet can be "positive" in tone while opposing the decision). I'd validate sentiment against a hand-labeled sample using precision and recall before trusting it.
- **Choosing topic count.** I'd use topic coherence scores rather than k-means elbow plots to choose the number of LDA topics.
- **Hashtags and engagement.** I'd handle hashtags separately and weight by engagement to separate what was *said* from what *spread*.
- This project's sentiment limitations are the same problem I later worked on professionally at Pinterest, where I replaced lexicon-based sentiment with an LLM scoring system validated against a labeled test set.

---

## Repository Structure

```
biostats_masters/
├── README.md
├── BSTT_528_Final__Copy_.pdf      # MI mortality prediction
├── BSTT_FINAL_TEXT___527.pdf      # Cervical cancer risk factors
└── BSTT_426_FINAL_TEXT.pdf        # Roe v. Wade Twitter analysis
```

<!-- TODO: once code is moved out of the PDFs, add:
├── mi_mortality/analysis.R
├── cervical_cancer/analysis.R
└── roe_v_wade/analysis.ipynb
-->

## About Me

I'm Michelle Carrizosa, a data scientist with 10+ years in analytics, focused on measuring AI products, retention, and causal inference. See my [current portfolio project](https://github.com/mcarrizosa44/ai-care-measurement) for more recent work. [LinkedIn](https://www.linkedin.com/in/michellecarrizosa/)
