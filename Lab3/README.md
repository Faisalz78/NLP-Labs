# Lab 3 — N-Grams and Tweet Generation

**Course:** Natural Language Processing (NLP)  
**Student:** Faisal AL Zahrani  
**Student ID:** 2230000363

## Overview

This lab introduces unigram, bigram, and trigram language models, token frequencies, boundary padding, maximum likelihood estimation (MLE), and perplexity. The practical task trains a bigram model on the specified tweet dataset, generates example tweets, and evaluates the model on held-out data.

The completed [Lab3.ipynb](Lab3.ipynb) preserves the original lab order and includes explanations, code, and saved outputs. All **24 code cells** were executed successfully.

## Files

```text
Lab3/
├── Lab3.ipynb
├── README.md
├── .gitattributes
└── data/
    ├── pakistan_tweets_v2.zip
    └── Random Tweets from Pakistan- Cleaned- Anonymous.csv
```

| File | Purpose |
|---|---|
| [Lab3.ipynb](Lab3.ipynb) | Completed lab with executed results. |
| [Dataset ZIP](data/pakistan_tweets_v2.zip) | Original Kaggle archive; the notebook reads the CSV directly from this file. |
| [Dataset CSV](data/Random%20Tweets%20from%20Pakistan-%20Cleaned-%20Anonymous.csv) | Extracted original data for direct inspection or reuse. |
| `.gitattributes` | Preserves dataset bytes during Git checkouts, including original CSV line endings. |

## Dataset

- **Source:** [Anonymous – Random Tweets, by Adeel Zafar](https://www.kaggle.com/datasets/adizafar/large-random-tweets-from-pakistan).
- **Version:** 2.
- **Original size:** 202,202 rows and 7 columns.
- **Text column:** `full_text`.
- **Archive size:** 11,700,972 bytes.
- **Extracted CSV size:** 40,109,612 bytes.
- **Source license label:** Data files © Original Authors.

The included files are the original archive and the unchanged extracted CSV. Preprocessing takes place in memory; it does not overwrite the source data.

The notebook checks this SHA-256 fingerprint of the CSV:

```text
54c8a9c5259c3a0ce2fe6b370fb82d2ab5c8e1ebc598f3238f8a5814325c62e9
```

The source contains some invalid UTF-8 bytes. The loader uses replacement characters to identify affected tweets, then excludes those tweets before training. Valid Unicode text, including Urdu, is preserved.

## Completed Tasks

| Task | Implementation |
|---|---|
| 1 — Load data | Load the specified Kaggle dataset and verify its fingerprint. |
| 2 — Preprocess | Remove entire hashtags, standalone RT markers, websites, mentions, and emojis; normalize case and whitespace; tokenize; remove missing, damaged, empty, and duplicate entries. |
| 3 — Train and generate | Train `MLE(2)` with sentence-boundary padding, then generate five reproducible tweets. |
| 4 — Evaluate | Measure perplexity on held-out bigrams, report unseen events and unknown words, and add a separately labeled Laplace comparison. |
| 5 — Bigram probability | Calculate `P(is \| pakistan)` from training counts and verify it against the model score. |
| 6 — Word perplexity | Calculate perplexity for `pakistan` without context, plus clearly labeled start-of-tweet and complete one-word-tweet interpretations. |

The introductory unigram, bigram, trigram, padding, MLE, and perplexity examples are included. The padding question is answered, and the original examples' probability-direction comments and substring-based word counting are corrected.

## How to Run

1. Open a terminal in the **Lab3** folder:

   ```powershell
   cd "C:\Users\faisa\Documents\GitHub\NLP-Labs\Lab3"
   ```

2. Install the required packages if needed:

   ```powershell
   python -m pip install jupyterlab nltk==3.9.2 emoji==2.16.0 "pandas>=2.2,<3" "jinja2>=3.1,<4"
   ```

3. Launch JupyterLab:

   ```powershell
   python -m jupyterlab
   ```

4. Open `Lab3.ipynb`, select a Python 3 kernel, and use **Restart Kernel and Run All Cells**.

You can also open the notebook in VS Code with its Python and Jupyter extensions. Ensure the notebook's working directory is `Lab3` so `data/pakistan_tweets_v2.zip` resolves correctly.

The notebook uses the included ZIP archive, so it does **not** need to download the dataset again. Once the Python dependencies are installed, the lab can run without internet access. If the ZIP is missing, the notebook automatically downloads version 2 from Kaggle. It includes manual recovery instructions if that download is unavailable.

No NLTK corpus download is required: the introductory examples use `word_tokenize(..., preserve_line=True)`, and the tweet task uses `TweetTokenizer`.

## Data Preparation and Split

| Stage | Count |
|---|---:|
| Original rows | 202,202 |
| Missing texts removed | 51 |
| Damaged texts removed | 6,538 |
| Empty token sequences removed | 21,438 |
| Duplicate token sequences removed | 53,903 |
| Unique usable tweets | 120,272 |
| Training tweets | 96,217 |
| Test tweets | 24,055 |

The split uses an 80/20 ratio with random seed **42**. Identical token sequences are removed before splitting, and the vocabulary and model counts come only from the training set. Each tweet is padded separately, preventing bigrams from crossing tweet boundaries.

Stopwords such as `is` are retained. Punctuation-only and number-only tokens are discarded. Hashtag removal deletes the complete hashtag, including its word.

## Results

These values come from the saved notebook run using the included dataset and the documented preprocessing:

| Metric | Result |
|---|---:|
| Training word tokens | 1,670,492 |
| Test word tokens | 417,148 |
| Vocabulary size, including boundary and unknown tokens | 118,036 |
| Evaluated test bigrams | 441,203 |
| Zero-probability MLE events | 156,151 (35.39%) |
| Out-of-vocabulary test word tokens | 17,889 (4.29%) |
| Held-out MLE perplexity | ∞ |
| Held-out Laplace perplexity | 19,697.1588 |
| `C(pakistan, is)` | 282 |
| `C(pakistan)` | 6,445 |
| `P(is \| pakistan)` | 0.04375485 (4.3755%) |
| Perplexity of `pakistan` without context | 289.049806 |
| Perplexity of `pakistan` after `<s>` | 152.725397 |
| Perplexity of the complete one-word tweet | 35.298280 |

**Why is MLE perplexity infinite?** Unsmoothed MLE assigns zero probability to unseen bigrams. Even one zero-probability test event makes perplexity infinite. This is an expected model limitation, not an execution error. The Laplace model is a supplementary smoothed comparison; it does not replace the required MLE model.

**What does single-word perplexity mean?** With no preceding context, the reported answer is `1 / P(pakistan)`, using the unigram counts stored by the trained model. Its denominator includes the training boundary tokens. Predicting the word after `<s>` or scoring the whole one-word tweet uses different events, so those values are labeled separately.

**Generated text:** Five examples use fixed seeds 42–46. Generation stops at `</s>` or a limit of 30 tokens / 280 characters. The source is multilingual, and the model conditions on only one preceding word, so generated tweets may mix languages and lack long-range coherence.

## References

- [Kaggle dataset](https://www.kaggle.com/datasets/adizafar/large-random-tweets-from-pakistan)
- [NLTK language models](https://www.nltk.org/api/nltk.lm.html)
- [NLTK tokenization](https://www.nltk.org/api/nltk.tokenize.html)
- [emoji documentation](https://emoji-python.readthedocs.io/en/stable/)
