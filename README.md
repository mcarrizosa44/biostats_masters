# Biostatistics M.S. Projects

Projects from my M.S. in Biostatistics at the University of Illinois Chicago, covering clinical prediction, risk-factor analysis, and text analytics. Each folder has its own README with the data, methods, and results.

| Project | Question | Methods | Tools |
| --- | --- | --- | --- |
| [ICU Mortality Prediction](icu-mortality-prediction/) | Can the first 48 hours of ICU data predict in-hospital death? | Gradient boosting, XGBoost, penalized logistic regression, SHAP | Python |
| [Predicting Death After Myocardial Infarction](Predicting%20Death%20After%20Myocardial%20Infarction/) | Which factors predict death after a heart attack? | _[methods]_ | _[tools]_ |
| [Cervical Cancer Risk Factors](Cervical%20Cancer%20Risk%20Factors/) | Which patient factors predict a positive cervical biopsy? | Decision tree, logistic regression, naive Bayes, GBM | R |
| [Roe v. Wade Public Discussion](Roe%20vs.%20Wade%20Public%20Discussion%20Sentiment%20Twitter%20Data/) | How did public sentiment on Twitter shift around Roe v. Wade? | Sentiment analysis, _[methods]_ | _[tools]_ |

## Projects

### [Predicting In-Hospital Mortality from the First 48 Hours of ICU Data](icu-mortality-prediction/)

Compared eight classifiers on 12,000 ICU stays from the PhysioNet/Computing in Cardiology Challenge 2012. Gradient boosting reached a ROC-AUC of about 0.84. However, accuracy barely beat a model that predicts everyone survives, and recall for deaths was 0.11 at the default threshold. The revised version reframes evaluation around discrimination, threshold choice, and calibration. [Paper (PDF)](icu-mortality-prediction/icu_mortality.pdf)

### [Predicting Death After Myocardial Infarction](Predicting%20Death%20After%20Myocardial%20Infarction/)

_[One to three sentences: the data, the main method, and the headline result with a number.]_

### [Cervical Cancer Risk Factors](Cervical%20Cancer%20Risk%20Factors/)

Modeled positive biopsy results for 858 patients using the UCI Cervical Cancer (Risk Factors) dataset, where only about 6% of biopsies were positive. A decision tree guided feature selection, and logistic regression, naive Bayes, and GBM were then compared. The Schiller iodine test was the strongest predictor, followed by age and years of hormonal contraceptive use.

### [Roe v. Wade Public Discussion: Twitter Sentiment](Roe%20vs.%20Wade%20Public%20Discussion%20Sentiment%20Twitter%20Data/)

_[One to three sentences: the tweet sample and time window, the sentiment method, and the main finding.]_

## Skills demonstrated

- **Clinical prediction with imbalanced outcomes:** threshold choice, recall versus precision, and baseline comparisons
- **Feature engineering for irregular time-series data** and handling missingness
- **Model interpretation** with SHAP, feature importance, and odds ratios
- **Text and sentiment analysis** of social media data
- **Tools:** Python (pandas, scikit-learn, XGBoost), R, and LaTeX

## About

**Michelle Carrizosa, M.S.**: M.S. Biostatistics, University of Illinois Chicago (2023); B.S. Mathematics, Cal Poly San Luis Obispo. Senior Product Analyst working on measurement, experimentation, retention, and LLM-based analytics.

[LinkedIn](https://www.linkedin.com/in/michellecarrizosa) · [Email](mailto:mcarrizosa4@gmail.com)
