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
