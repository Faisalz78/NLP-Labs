# Lab 4 — Classification and Evaluation in NLP

**Course:** Natural Language Processing (NLP)  
**Student:** Faisal AL Zahrani  
**Student ID:** 2230000363

## Overview

This lab classifies review sentiment as **negative**, **neutral**, or **positive**. It includes the original Disneyland example using a linear-kernel Support Vector Machine and all six Amazon review tasks using Multinomial Naive Bayes.

The completed [Lab4.ipynb](Lab4.ipynb) contains the explanations, executed outputs, classification reports, and numeric and visual confusion matrices. All **19 code cells** were executed successfully using the full source datasets before documented cleaning and deduplication.

## Files

```text
Lab4/
├── Lab4.ipynb
├── README.md
├── requirements.txt
├── .gitattributes
├── .gitignore
└── data/
    ├── disneyland_reviews_v1.zip
    └── amazon_unlocked_phones_v1.zip
```

The original CSV files remain inside their ZIP archives and are read directly by the notebook. No extraction is needed. The Amazon CSV is approximately 132 MB uncompressed; retaining the original 34 MB ZIP avoids duplicating it in the repository.

## Datasets

| Dataset | Original rows | CSV inside the archive | Source |
|---|---:|---|---|
| Disneyland Reviews | 42,656 | `DisneylandReviews.csv` | [Kaggle, version 1](https://www.kaggle.com/datasets/arushchillar/disneyland-reviews) |
| Amazon Reviews: Unlocked Mobile Phones | 413,840 | `Amazon_Unlocked_Mobile.csv` | [Kaggle, version 1](https://www.kaggle.com/datasets/PromptCloudHQ/amazon-reviews-unlocked-mobile-phones) |

Both sources are labeled **CC0: Public Domain** on Kaggle. Original data bytes are preserved. The notebook verifies each CSV fingerprint before loading it.

- Disneyland CSV SHA-256: `3c5eb28455a5156bbe337b3768fdd422a3311c1a91cb33214682bf80419e885c`
- Amazon CSV SHA-256: `097abeefe303816e0f5e9c9ff380adb25910cf46307d2c104911eae8d0304e76`

Disneyland uses the lab's Latin-1 decoding. Amazon is valid UTF-8. The actual Disneyland branch column is named `Branch`.

## Sentiment Labels

| Star rating | Sentiment |
|---|---|
| 1 or 2 | Negative |
| 3 | Neutral |
| 4 or 5 | Positive |

Ratings create the target labels; they are not included as input features. Only review text is used for prediction. These rating-derived labels are proxies for sentiment.

## Completed Tasks

| Task | Solution |
|---|---|
| 1 — Load and preprocess | Load the complete Amazon dataset and apply five explicit preprocessing steps. |
| 2 — Train/test split | Use a stratified 80/20 split with seed 42 and verify that cleaned texts do not overlap. |
| 3 — TF-IDF | Fit on training reviews only; use unigrams/bigrams, `min_df=2`, and up to 20,000 features. |
| 4 — Naive Bayes | Train `MultinomialNB(alpha=1.0)` on the sparse TF-IDF matrix. |
| 5 — Evaluate | Report accuracy, balanced accuracy, macro-F1, weighted-F1, and per-class metrics; compare to a majority-class baseline. |
| 6 — Confusion matrix | Print the numeric matrix and labeled table, then plot counts and row-normalized proportions. |

The introductory Disneyland workflow is also fully completed using `SVC(kernel="linear")` and 1,000 TF-IDF features.

## Five Preprocessing Steps

1. Decode HTML entities and remove HTML tags.
2. Remove URLs and email addresses.
3. Lowercase and expand negative contractions, preserving negation.
4. Replace punctuation, digits, and special characters with spaces.
5. Tokenize, filter common stopwords while retaining negation words, and normalize whitespace.

Examples such as **not good** retain their negative meaning. The stopword list is fixed and does not depend on the test data.

Missing or invalid required fields and empty cleaned texts are removed. All rows for a cleaned text with conflicting sentiment labels are removed, then remaining duplicate texts are reduced to one copy. Missing brands, prices, or vote counts do not cause otherwise usable reviews to be dropped.

## Run the Notebook

Use **Python 3.11 or later**. In a terminal:

```powershell
cd "C:\Users\faisa\Documents\GitHub\NLP-Labs\Lab4"
python -m pip install -r requirements.txt
python -m jupyterlab
```

Open `Lab4.ipynb`, select a Python 3 kernel, and choose **Restart Kernel and Run All Cells**. VS Code with its Python and Jupyter extensions is also suitable. Ensure the working directory is `Lab4`.

Both dataset archives are included, so no dataset download or extraction is necessary. Once dependencies are installed, reruns can work offline. If an archive is missing, the notebook attempts to download the documented Kaggle version. No NLTK resource download is needed.

The SVM example may take several minutes depending on the computer. The saved outputs can be inspected immediately.

## Data Preparation

| Amazon stage | Count |
|---|---:|
| Original rows | 413,840 |
| Missing reviews removed | 70 |
| Invalid ratings removed | 0 |
| Empty cleaned reviews removed | 1,250 |
| Rows with conflicting labels removed | 63,951 |
| Repeated texts removed | 196,863 |
| Usable unique reviews | 151,706 |
| Training reviews | 121,364 |
| Test reviews | 30,342 |

For Disneyland, 42,630 unique usable reviews remain, split into 34,104 training and 8,526 test reviews.

## Results

The following values are from the executed notebook with the included dataset versions and documented preprocessing.

### Disneyland SVM

| Metric | Value |
|---|---:|
| Accuracy | 0.8455 (84.55%) |
| Macro-F1 | 0.5858 |

### Amazon Naive Bayes

| Metric | Multinomial Naive Bayes | Majority-class baseline |
|---|---:|---:|
| Accuracy | 0.8320 | 0.6244 |
| Balanced accuracy | 0.6008 | 0.3333 |
| Macro-F1 | 0.5849 | 0.2563 |
| Weighted-F1 | 0.7964 | 0.4801 |

The TF-IDF representation has **20,000 features**. **132** test reviews contain no features in the training vocabulary; these are retained in the evaluation.

| Sentiment | Precision | Recall | F1 | Support |
|---|---:|---:|---:|---:|
| negative | 0.7875 | 0.8225 | 0.8046 | 8,621 |
| neutral | 0.3550 | 0.0256 | 0.0477 | 2,774 |
| positive | 0.8555 | 0.9543 | 0.9022 | 18,947 |

### Confusion Matrix

Rows are **actual** labels; columns are **predicted** labels.

| Actual / Predicted | Negative | Neutral | Positive |
|---|---:|---:|---:|
| negative | 7,091 | 52 | 1,478 |
| neutral | 1,126 | 71 | 1,577 |
| positive | 788 | 77 | 18,082 |

The diagonal contains correct predictions. Row-normalized plots in the notebook show each class's recall and errors.

## Interpretation

The lowest class F1 is for **neutral** reviews. Accuracy alone can hide weak minority-class performance, so macro-F1, balanced accuracy, and the per-class report are included. Neutral and mixed reviews can be hard to distinguish using rating-derived labels and a simple bag-of-words classifier.

The test set is held out from vocabulary fitting, IDF estimation, and classifier training. Fixed settings are specified before evaluation; no test-set tuning is performed. Duplicate handling means these results describe unique, unambiguous cleaned review texts. The split does not hold out entire products, and the results should not be read as performance on unseen products or future reviews.

## References

- [Disneyland Reviews](https://www.kaggle.com/datasets/arushchillar/disneyland-reviews)
- [Amazon Reviews: Unlocked Mobile Phones](https://www.kaggle.com/datasets/PromptCloudHQ/amazon-reviews-unlocked-mobile-phones)
- [TF-IDF vectorizer](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html)
- [Multinomial Naive Bayes](https://scikit-learn.org/stable/modules/generated/sklearn.naive_bayes.MultinomialNB.html)
- [Classification report](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.classification_report.html)
