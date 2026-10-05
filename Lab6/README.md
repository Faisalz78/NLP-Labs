# Lab 6 — Deep Learning for NLP

**Course:** Natural Language Processing (NLP)  
**Student:** Faisal AL Zahrani  
**Student ID:** 2230000363

## Overview

The completed [Lab6.ipynb](Lab6.ipynb) classifies Yelp reviews as **negative (0)** or **positive (1)** with two PyTorch pipelines: TF-IDF features and averaged Word2Vec embeddings. It preserves the original lab order, completes both tasks, and includes saved results and explanations.

All **17 code cells** ran successfully on Python 3.12.14 using the CPU. The source notebook's **27 cells** are retained in order, with the blank solutions completed and the worked example corrected.

## Files

```text
Lab6/
├── Lab6.ipynb
├── README.md
├── requirements.txt
├── .gitattributes
├── .gitignore
└── data/
    └── 07-yelp-dataset.txt
```

The dataset is the supplied file, copied without modification. No dataset or pretrained-model download is required. The notebook uses `data/07-yelp-dataset.txt`, with a fallback to the same filename beside the notebook.

## Completed Work

| Part | Completed requirements |
|---|---|
| Worked example, sections 1–10 | Load and inspect Yelp data; preprocess; split; fit TF-IDF; create tensors; build and train a neural network; calculate accuracy, classification report, and confusion matrix. |
| Task 1 — Word2Vec | Tokenize training and test reviews separately; train Skip-gram on training reviews only using `vector_size=200`, `window=5`, `min_count=1`, `sg=1`; average known word vectors per review; convert to PyTorch tensors. |
| Task 2 — Deeper network | Build **200 → 128 → 64 → 32 → 1** with ReLU after all three hidden layers; use `BCEWithLogitsLoss`, Adam at **0.001**, and **15 epochs**; calculate test accuracy. |

Additional outputs include training-loss curves, confusion-matrix plots, a pipeline comparison, unseen-word counts, and a brief interpretation of the actual results.

## Corrections to the Worked Example

1. The preprocessing call was indented inside the function after `return`, making it unreachable. It is now executed outside the function.
2. The training loop now calls `optimizer.zero_grad()` before every update to prevent gradients accumulating between epochs.
3. The split now includes `stratify=y`, matching the notebook's explanation.
4. Seeds and deterministic CPU execution are specified for repeatability.

The supplied preprocessing rules are preserved: lowercase, remove non-letter/non-whitespace characters, and collapse whitespace. This removes apostrophes rather than expanding contractions. Stopwords are retained. Both TF-IDF fitting and Word2Vec training use only training reviews.

## Data and Split

| Measure | Value |
|---|---:|
| Reviews | 1,000 |
| Negative / positive | 500 / 500 |
| Missing fields | 0 |
| Training reviews | 800: 400 negative, 400 positive |
| Test reviews | 200: 100 negative, 100 positive |
| Repeated cleaned reviews beyond the first occurrence | 5 |
| Identical cleaned review texts shared across splits | 0 |

All 1,000 rows are retained. The 80/20 stratified split uses `random_state=42`; both pipelines receive the same split. The notebook verifies that training and test row IDs are disjoint and that no identical cleaned text crosses this split.

## Model Settings

| Setting | TF-IDF pipeline | Word2Vec pipeline |
|---|---|---|
| Input features | Up to 1,000 TF-IDF features | Mean of 200-dimensional word vectors |
| Hidden layers | 64, 32 | 128, 64, 32 |
| Activation | ReLU | ReLU |
| Output | One logit | One logit |
| Loss | BCEWithLogitsLoss | BCEWithLogitsLoss |
| Optimizer | Adam | Adam |
| Learning rate | 0.01 | 0.001 |
| Classifier epochs | 10 | 15 |
| Training batch | All 800 training reviews | All 800 training reviews |
| Positive prediction | Sigmoid score ≥ 0.5 | Sigmoid score ≥ 0.5 |

Each classifier epoch makes one full-batch optimizer update, matching the worked example. Sigmoid is applied only during evaluation; it is already included numerically inside `BCEWithLogitsLoss` during training.

Word2Vec uses the task's four required settings, plus explicit `epochs=5` (Gensim's default), `seed=42`, `workers=1`, and a stable CRC32 hash function. Training is unsupervised on the 800 training reviews; labels are used by the classifier only. Review embeddings are fixed while training the classifier.

The training vocabulary contains **1,787** words from **8,620** token occurrences. Test reviews contain **2,157** token occurrences, of which **281 (13.0%)** are unseen during training. Unseen words are skipped when averaging; a review with no known words receives a zero vector. There are **0** such test reviews in this dataset.

## Actual Results

| Pipeline | Training accuracy | Test accuracy | Test loss |
|---|---:|---:|---:|
| TF-IDF | 96.875% | **75.5%** | 0.496936 |
| Mean Word2Vec | 50.500% | **49.0%** | 0.693391 |
| Majority-class baseline | 50.0% | 50.0% | — |

Confusion matrices use **rows = actual class** and **columns = predicted class**, ordered **negative, positive**:

```text
TF-IDF                 Word2Vec
[[67, 33],             [[ 98,  2],
 [16, 84]]              [100,  0]]
```

The Word2Vec pipeline did **not** outperform the majority-class baseline in this run. Its training accuracy is also near 50%, and it predicts almost every test review as negative. This indicates that the classifier has learned little class separation under these settings. Its low accuracy is reported as measured.

Word2Vec was trained from scratch on only 800 short reviews. Mean pooling loses word order, including negation order, and the classifier receives only 15 full-batch updates. These are plausible contributors to weak performance; this experiment does not isolate their individual effects. The TF-IDF model's training-to-test gap also shows limited generalization.

This comparison changes the representation, network architecture, learning rate, and epoch count, so it cannot establish that one representation is universally better. No test-driven tuning was performed. Future changes should be selected on a separate validation split before evaluating the held-out test set.

## Run on Windows

Use **Python 3.12**, matching the tested environment. The dependency file pins the numerical libraries and selects a CPU build of PyTorch, so a GPU is not needed.

```powershell
cd "C:\Users\faisa\Documents\GitHub\NLP-Labs\Lab6"
py -3.12 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m ipykernel install --user --name nlp-lab6 --display-name "Python 3.12 (NLP Lab6)"
.\.venv\Scripts\python.exe -m jupyterlab
```

Open `Lab6.ipynb`, select **Python 3.12 (NLP Lab6)**, and choose **Restart Kernel and Run All Cells**. VS Code can use the same environment. Keep the working directory set to `Lab6`.

The setup cell checks for dependencies and explains how to install missing packages. Once dependencies are installed, the entire lab runs offline. Existing outputs can be reviewed without rerunning. Numerical results may differ with other package versions or platforms.

## Verification and References

- All 17 code cells executed without error outputs.
- Both original task prompts and all original cell positions were retained.
- Train/test shapes, required model settings, unseen-word handling, and confusion-matrix accuracy were checked.
- The dataset checksum is verified before loading: SHA-256 `c76468b7b5c6e56a0804d728345c5f84aa2142ddb214420f61cc9cfd4c00d2ea`.
- The source's two Google Drive illustrations could not be retrieved. Their links remain in the notebook, alongside a self-contained explanation of both architectures.
- Dataset source: the file supplied with this lab. No additional license is assigned here.
- [PyTorch training loop and gradient reset](https://docs.pytorch.org/tutorials/beginner/basics/optimization_tutorial.html)
- [PyTorch BCEWithLogitsLoss](https://docs.pytorch.org/docs/2.14/generated/torch.nn.BCEWithLogitsLoss.html)
- [Gensim Word2Vec](https://radimrehurek.com/gensim/models/word2vec.html)
