# Lab 5 — Text Representation

**Course:** Natural Language Processing (NLP)  
**Student:** Faisal AL Zahrani  
**Student ID:** 2230000363

## Overview

This lab covers TF-IDF document representations, cosine similarity, and Skip-gram Word2Vec embeddings. The completed [Lab5.ipynb](Lab5.ipynb) includes all five tasks, the introductory examples, explanations, and saved outputs.

All **23 code cells** were executed successfully. The Simpsons cleaning function and the four requested model settings are preserved exactly.

## Files

```text
Lab5/
├── Lab5.ipynb
├── README.md
├── requirements.txt
├── .gitattributes
├── .gitignore
└── data/
    ├── simpsons_script_lines.csv
    └── fake_and_real_news_v1.zip
```

The Simpsons CSV is the file supplied with the lab. The news ZIP is the original version-1 Kaggle archive; the notebook reads **True.csv** directly from it. **Fake.csv** is also present in the archive but is not used by the lab.

## Completed Tasks

| Task | Implementation |
|---|---|
| 1 — Cosine similarity | Represent the four sentences with default TF-IDF; show the full similarity matrix, all six distinct pairs, and a heatmap. |
| 2 — TF-IDF | Show document-term weights and IDF values; rank the top terms and report all terms tied for each document's maximum. |
| 3 — Simpsons Skip-gram | Clean and tokenize `spoken_words`; train the specified 100-dimensional Word2Vec model. |
| 4 — Similar words | Retrieve ten neighbors and cosine scores for `homer`, `marge`, and `bart`. |
| 5 — Odd word | Apply `wv.doesnt_match()` to all three supplied lists and verify the answer using vector-centroid similarity. |

The original task numbers its three odd-word lists **1, 3, and 4**. All three are answered; no extra list is missing from the source.

The news example is also completed: it reports similarity between **provide** and **program**, then the ten most similar words to **program**.

## Run the Notebook

Use **Python 3.12**, the version used for validation. The Python 3.14 environment used for earlier labs did not have a compatible prebuilt Gensim 4.4.0 package, so select a Python 3.12 kernel for this lab.

With Python 3.12 installed on Windows:

```powershell
cd "C:\Users\faisa\Documents\GitHub\NLP-Labs\Lab5"
py -3.12 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m ipykernel install --user --name nlp-lab5 --display-name "Python 3.12 (NLP Lab5)"
.\.venv\Scripts\python.exe -m jupyterlab
```

Open `Lab5.ipynb`, select **Python 3.12 (NLP Lab5)**, then choose **Restart Kernel and Run All Cells**. VS Code can use the same interpreter/kernel. Keep the working directory set to `Lab5`.

Once the dependencies are installed, both included datasets can be loaded offline. No NLTK resource download or ZIP extraction is needed. If the news archive is missing, the notebook downloads the specified Kaggle version. The Simpsons file is supplied locally and is not substituted with another dataset.

Word2Vec training takes several minutes depending on the computer. Existing results are already saved for immediate review.

## Methods and Reproducibility

### TF-IDF

The code uses scikit-learn's default smoothed IDF and L2 normalization. Words are lowercased, punctuation is ignored, and the default token pattern excludes one-character tokens. Stopwords remain in the TF-IDF tasks, matching the introductory examples.

Repeated or rare terms can receive high weights; a high TF-IDF score is not an independent measure of semantic importance. TF-IDF ignores word order and is a sparse document representation, distinct from a learned dense word embedding.

### Word2Vec

| Setting | Value |
|---|---|
| Architecture | Skip-gram (`sg=1`) |
| Minimum word count | 1 |
| Vector dimensions | 100 |
| Maximum context window | 5 |
| Epochs | 5 |
| Random seed | 42 |
| Training workers | 1 |
| Hash function | Stable CRC32-based function |

The original four model settings are unchanged. Seed, worker count, epoch count, and stable hashing make reruns repeatable with the same software and input data. Different library versions or platforms may produce different numerical results.

For news, case and punctuation are retained as in the original example, with one article per token sequence. For Simpsons, the exact supplied `clean_text` function lowercases text and removes digits and the specified characters. NLTK tokenizes each line using `preserve_line=True`.

Only missing or empty dialogue is removed. Duplicate lines and stopwords are retained as training observations. Some punctuation remains because the supplied cleaning function removes only its specified characters.

## Dataset Summary

| Measure | Simpsons | News (True.csv) |
|---|---:|---:|
| Source rows | 158,314 | 21,417 |
| Missing dialogue rows removed | 26,459 | — |
| Empty token sequences removed | 16 | 1 |
| Sequences used | 131,839 | 21,416 |
| Tokens | 1,479,360 | 9,033,146 |
| Vocabulary entries | 44,282 | 114,680 |

The model uses the **spoken_words** column. Missing character names do not cause otherwise valid dialogue to be removed.

## Saved Results

### Task 1 — Cosine Similarity

Sentence IDs follow the order in the task. Rows and columns use the same order.

| Sentence | S1 | S2 | S3 | S4 |
|---|---:|---:|---:|---:|
| S1 | 1.000000 | 0.646926 | 0.307772 | 1.000000 |
| S2 | 0.646926 | 1.000000 | 0.225240 | 0.646926 |
| S3 | 0.307772 | 0.225240 | 1.000000 | 0.307772 |
| S4 | 1.000000 | 0.646926 | 0.307772 | 1.000000 |

**S1 and S4 score 1.0** because their represented words are identical after tokenization, although their word order and punctuation differ.

### Task 2 — Highest TF-IDF Weights

| Document | Terms tied for highest weight | Weight |
|---|---|---:|
| D1 | of, science | 0.488098 |
| D2 | best, courses, this | 0.400294 |
| D3 | data | 0.641055 |

The notebook also includes the full feature matrix, IDF values, and the top five nonzero terms per document. Function words can score highly because stopwords are deliberately retained and term repetition contributes to TF-IDF.

### Task 4 — Nearest Words

The first three neighbors from each ten-word output are summarized here.

| Query | Neighbors (cosine similarity) |
|---|---|
| homer | abe (0.8901); marge (0.8633); bart (0.8322) |
| marge | homer (0.8633); abe (0.8485); sweetie (0.8321) |
| bart | milhouse (0.8837); lisa (0.8595); grampa (0.8544) |

### Task 5 — Odd Words

| Original item | Word list | Model-selected odd word |
|---|---|---|
| 1 | jimbo, milhouse, kearney | milhouse |
| 3 | nelson, bart, milhouse | nelson |
| 4 | homer, patty, selma | homer |

These answers are taken directly from the trained model. The notebook verifies them by comparing each normalized word vector with the group's mean direction. Contextual similarity is not a factual judgment about the characters, and scores are not probabilities.

### Introductory Examples

- Two-document TF-IDF cosine similarity: **0.327871**.
- News Word2Vec similarity of **provide** and **program**: **0.432902**.

## Source Integrity and References

The notebook verifies the original dataset fingerprints:

- Simpsons CSV SHA-256: `3e726c08f74fb6be3dc28f7b10f156fb1b0a2778e774ebe7b11d6ab072176ae9`
- True.csv SHA-256: `ba0844414a65dc6ae7402b8eee5306da24b6b56488d6767135af466c7dcb2775`

The five original Google Drive illustration links are retained as references. Their images could not be retrieved during preparation; the notebook's explanations, tables, and computed heatmap do not depend on those external images.

- [Fake and Real News dataset](https://www.kaggle.com/datasets/clmentbisaillon/fake-and-real-news-dataset), version 1, creator **clmentbisaillon**, license **CC BY-NC-SA 4.0**. The included source archive is unchanged.
- **Simpsons CSV:** supplied with the lab; no additional license is assigned here.
- [Scikit-learn TF-IDF](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfTransformer.html)
- [Gensim Word2Vec](https://radimrehurek.com/gensim/models/word2vec.html)
- [Gensim vector queries](https://radimrehurek.com/gensim/models/keyedvectors.html)
