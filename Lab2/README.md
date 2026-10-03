# Lab 2: Text Pre-processing and Regular Expressions

## Student Information

- **Name:** Faisal AL Zahrani


## Overview

This lab introduces essential techniques for **text pre-processing** and **regular expressions (Regex)** in Natural Language Processing (NLP).

The lab demonstrates how raw text can be searched, cleaned, normalized, and tokenized before being used in NLP or machine learning applications. It also provides practical experience with Python's `re` module, NLTK, spaCy, and pandas.

## Learning Objectives

By completing this lab, the following concepts are practiced:

- Using Regular Expressions for pattern matching and text manipulation
- Searching and extracting patterns from text
- Tokenizing text into sentences and words
- Converting text to lowercase
- Applying stemming techniques
- Applying lemmatization
- Working with stop words
- Comparing tokenization using spaCy and NLTK
- Analyzing hashtags from a real Twitter dataset

## Topics Covered

### 1. Regular Expressions

Python's built-in `re` module is used to work with regular expressions.

Important functions covered in the lab include:

- `re.search()` — finds the first occurrence of a pattern anywhere in a string
- `re.match()` — checks for a pattern only at the beginning of a string
- `re.findall()` — returns all matches
- `re.sub()` — replaces matched patterns
- `re.compile()` — creates a reusable regular expression pattern
- `re.split()` — splits text based on a regular expression pattern

Examples of useful Regex patterns:

```python
r"\d+"      # One or more digits
r"#\w+"     # Hashtags
r"[a-zA-Z]+" # Alphabetic words
```

### 2. Text Pre-processing

The lab covers common NLP pre-processing steps:

#### Tokenization

Breaking text into smaller units such as sentences or words.

Examples:

```python
nltk.sent_tokenize(text)
nltk.word_tokenize(text)
```

#### Lowercasing

Converting text into lowercase to reduce unnecessary differences between words such as `Book`, `BOOK`, and `book`.

```python
text.lower()
```

#### Stemming

Reducing words to a simplified stem form.

The lab demonstrates:

- `PorterStemmer`
- `SnowballStemmer`

Example:

```text
running -> run
runs    -> run
```

#### Lemmatization

Reducing a word to its meaningful base form using linguistic information.

Example:

```text
taking -> take
am     -> be
```

The lab uses:

```python
WordNetLemmatizer
```

#### Stop Words

Stop words are common words that may carry little useful information in some NLP tasks.

Examples include:

```text
the, a, an, is, and, in
```

The lab demonstrates stop-word handling using both **spaCy** and **NLTK**.

---

## Lab Tasks

### Task 1 — Hashtag Extraction and Analysis

The Apple Twitter Sentiment dataset is analyzed using Regex.

The task includes:

- Loading the dataset with pandas
- Extracting hashtags using:

```python
r"#\w+"
```

- Counting the total number of hashtags
- Counting unique hashtags
- Finding the top 10 most frequently used hashtags

### Task 1 Results

The dataset used in this lab contains:

- **1,630 tweets**
- **1,616 total hashtag occurrences**
- **555 unique hashtags**

Top hashtags obtained from the analysis:

| Rank | Hashtag | Frequency |
|---:|---|---:|
| 1 | `#aapl` | 400 |
| 2 | `#apple` | 162 |
| 3 | `#iphone` | 38 |
| 4 | `#december` | 37 |
| 5 | `#iphone6` | 29 |
| 6 | `#stocks` | 15 |
| 7 | `#ipad` | 14 |
| 8 | `#iphone6plus` | 13 |
| 9 | `#iphone6s` | 13 |
| 10 | `#tech` | 12 |

### Task 2 — Using `re.compile()`

A compiled regular expression is created to match one or more digits:

```python
pattern = re.compile(r"\d+")
```

The pattern is then used to replace:

```text
This year is 2021
```

with:

```text
This year is 2022
```

`re.compile()` is useful when the same Regex pattern needs to be reused multiple times.

### Task 3 — Using `re.split()`

The following text is processed:

```text
a 11 b 2 3 c 4
```

The pattern:

```python
r"\d+"
```

is used to split the string wherever one or more digits appear.

### Task 4 — Tokenization Using spaCy and NLTK

The sentence:

```text
I'm enjoying the NLP course!
```

is tokenized using both:

- spaCy
- NLTK

For this sentence, both tokenizers produce:

```python
['I', "'m", 'enjoying', 'the', 'NLP', 'course', '!']
```

The task demonstrates that different NLP libraries may use different tokenization rules, even though their output is identical for this example.

---

## Technologies and Libraries

The lab uses:

- Python
- Jupyter Notebook
- pandas
- Regular Expressions (`re`)
- NLTK
- spaCy
- `en_core_web_sm`
- `collections.Counter`

## Installation

Install the required Python libraries:

```bash
pip install pandas nltk spacy
```

Install the spaCy English model:

```bash
python -m spacy download en_core_web_sm
```

Required NLTK resources can be downloaded inside Python:

```python
import nltk

nltk.download("punkt")
nltk.download("punkt_tab")
```

## Project Structure

```text
Lab2/
├── Lab2.ipynb
├── apple-twitter-sentiment-texts.csv
└── README.md
```

## Running the Lab

1. Place the dataset in the same directory as `Lab2.ipynb`.
2. Open the notebook using Jupyter Notebook, JupyterLab, or VS Code.
3. Run the cells from top to bottom.
4. Verify that the required libraries and NLP resources are installed.
5. Review the output generated for each task.

## Conclusion

This lab provides practical experience with fundamental NLP pre-processing techniques and regular expressions.

The exercises demonstrate how Python can be used to clean, normalize, tokenize, search, extract, and analyze text data. These techniques form an important foundation for more advanced NLP tasks such as sentiment analysis, text classification, information extraction, and language modeling.
