<div align="center">

# 🧠 Lab 2: Text Pre-processing & Regular Expressions

### Natural Language Processing (NLP)

**👨‍🎓 Student:** Faisal AL Zahrani  
**🆔 Student ID:** 2230000363

---

*Exploring essential text pre-processing techniques and Regular Expressions using Python, NLTK, spaCy, and pandas.*

</div>

## 📌 Overview

This lab introduces fundamental techniques used to prepare, clean, search, and analyze text data in **Natural Language Processing (NLP)**.

The lab combines two important areas:

- 🔎 **Regular Expressions (Regex)** for pattern matching, searching, extraction, replacement, and splitting.
- 🧹 **Text Pre-processing** for preparing raw text before using it in NLP or machine learning models.

The practical tasks also include analyzing hashtags from a real **Apple Twitter dataset**.

---

## 🎯 Learning Objectives

By completing this lab, we practice how to:

- 🔍 Search text using Regular Expressions
- 🧩 Extract specific patterns from text
- ✂️ Split text using Regex patterns
- 🔄 Replace matched text
- 📝 Tokenize sentences and words
- 🔡 Convert text to lowercase
- 🌱 Apply stemming
- 📖 Apply lemmatization
- 🧹 Work with stop words
- ⚖️ Compare spaCy and NLTK tokenization
- #️⃣ Analyze hashtags from real Twitter data

---

## 🧰 Technologies & Libraries

| Tool / Library | Purpose |
|---|---|
| 🐍 **Python** | Main programming language |
| 📓 **Jupyter Notebook** | Interactive development environment |
| 🐼 **pandas** | Dataset loading and analysis |
| 🔎 **re** | Regular Expressions |
| 📚 **NLTK** | Text processing and tokenization |
| ⚡ **spaCy** | Modern NLP processing |
| 🌐 **en_core_web_sm** | spaCy English language model |
| 🔢 **Counter** | Frequency counting |

---

## 🔎 Part 1 — Regular Expressions

Regular Expressions are used to identify and manipulate patterns inside text.

### Important Regex Functions

| Function | Description |
|---|---|
| `re.search()` | Finds the first match anywhere in the text |
| `re.match()` | Checks for a match at the beginning of the text |
| `re.findall()` | Returns all matching patterns |
| `re.sub()` | Replaces matched text |
| `re.compile()` | Creates a reusable Regex pattern |
| `re.split()` | Splits text using a Regex pattern |

### 🧪 Example Patterns

```python
r"\d+"       # One or more digits
r"#\w+"      # Hashtags
r"[a-zA-Z]+" # Alphabetic words
```

---

## 🧹 Part 2 — Text Pre-processing

Text pre-processing transforms raw text into a cleaner and more consistent format before further NLP analysis.

### 🧩 Tokenization

Tokenization breaks text into smaller units such as sentences or words.

```python
nltk.sent_tokenize(text)
nltk.word_tokenize(text)
```

Example:

```text
"I'm enjoying NLP!"
```

becomes:

```python
["I", "'m", "enjoying", "NLP", "!"]
```

---

### 🔡 Lowercasing

Lowercasing converts all letters into lowercase.

```python
text.lower()
```

Example:

```text
NLP → nlp
Book → book
BOOK → book
```

This helps reduce unnecessary differences between words.

---

### 🌱 Stemming

Stemming reduces words to a simplified stem, usually by removing suffixes.

The lab uses:

- `PorterStemmer`
- `SnowballStemmer`

Example:

```text
running → run
runs    → run
easily  → easili
```

> ⚠️ A stem does not always have to be a valid English word.

---

### 📖 Lemmatization

Lemmatization reduces a word to its meaningful base form using linguistic information.

Example:

```text
taking   → take
enjoying → enjoy
am       → be
```

The lab uses:

```python
WordNetLemmatizer
```

A key difference from stemming is that lemmatization can use **Part of Speech (POS)** information.

---

### 🧹 Stop Words

Stop words are common words that may provide little useful information for some NLP tasks.

Examples:

```text
the, a, an, is, and, in, to
```

The lab demonstrates stop-word handling using both:

- 📚 NLTK
- ⚡ spaCy

> 💡 Stop words should **not always be removed**.  
> For example, removing the word `not` from sentiment analysis can completely change the meaning of a sentence.

---

# 🧪 Lab Tasks

## #️⃣ Task 1 — Hashtag Extraction & Analysis

The Apple Twitter dataset is analyzed using Regex.

### Objectives

- Load the dataset using pandas
- Extract hashtags from tweets
- Count all hashtag occurrences
- Count unique hashtags
- Display the **Top 10 most frequently used hashtags**

The Regex pattern used is:

```python
r"#\w+"
```

### 📊 Dataset Summary

- 📝 **Tweets:** 1,630
- #️⃣ **Total hashtag occurrences:** 1,616
- 🔢 **Unique hashtags:** 555

### 🏆 Top 10 Hashtags

| Rank | Hashtag | Frequency |
|---:|---|---:|
| 🥇 1 | `#aapl` | 400 |
| 🥈 2 | `#apple` | 162 |
| 🥉 3 | `#iphone` | 38 |
| 4 | `#december` | 37 |
| 5 | `#iphone6` | 29 |
| 6 | `#stocks` | 15 |
| 7 | `#ipad` | 14 |
| 8 | `#iphone6plus` | 13 |
| 9 | `#iphone6s` | 13 |
| 10 | `#tech` | 12 |

---

## 🔁 Task 2 — Using `re.compile()`

The following text is processed:

```text
This year is 2021
```

A reusable Regex pattern is created:

```python
pattern = re.compile(r"\d+")
```

The year is then replaced:

```text
This year is 2021
        ↓
This year is 2022
```

### 💡 Why use `re.compile()`?

`re.compile()` creates a reusable Regex pattern object, which makes the code cleaner and more convenient when the same pattern is used multiple times.

---

## ✂️ Task 3 — Using `re.split()`

The following text is used:

```text
a 11 b 2 3 c 4
```

The pattern:

```python
r"\d+"
```

matches one or more digits.

The text is split whenever a number appears:

```python
['a ', ' b ', ' ', ' c ', '']
```

### 💡 What does `re.split()` do?

`re.split()` divides a string wherever the specified Regular Expression pattern is found.

---

## 🧩 Task 4 — Tokenization with spaCy & NLTK

The sentence:

```text
I'm enjoying the NLP course!
```

is tokenized using both **spaCy** and **NLTK**.

### ⚡ spaCy Output

```python
['I', "'m", 'enjoying', 'the', 'NLP', 'course', '!']
```

### 📚 NLTK Output

```python
['I', "'m", 'enjoying', 'the', 'NLP', 'course', '!']
```

### 🔍 Comparison

For this sentence, both libraries produce the same tokens.

However, spaCy and NLTK use different tokenization rules, so they may produce different results for more complex text.

### 🧠 What does `spacy.load()` do?

```python
spacy.load("en_core_web_sm")
```

loads spaCy's small English language model, which provides features such as:

- 🧩 Tokenization
- 🏷️ Part-of-Speech tagging
- 📖 Lemmatization
- 🧑‍💼 Named Entity Recognition

---

## ⚙️ Installation

Install the required libraries:

```bash
pip install pandas nltk spacy
```

Install the spaCy English model:

```bash
python -m spacy download en_core_web_sm
```

Download the required NLTK resources:

```python
import nltk

nltk.download("punkt")
nltk.download("punkt_tab")
```

---

## 📁 Project Structure

```text
Lab2/
│
├── 📓 Lab2.ipynb
├── 📊 apple-twitter-sentiment-texts.csv
└── 📄 README.md
```

---

## ▶️ How to Run

1. 📂 Place the dataset in the same folder as `Lab2.ipynb`.
2. 📓 Open the notebook using Jupyter Notebook, JupyterLab, or VS Code.
3. 📦 Install the required libraries.
4. ▶️ Run the notebook cells from top to bottom.
5. ✅ Review the output for each task.

---

## ✅ Conclusion

This lab provides practical experience with some of the most important foundations of **Natural Language Processing**.

Through the exercises, we practiced:

- 🔎 Pattern matching with Regex
- 🧹 Text cleaning and normalization
- 🧩 Tokenization
- 🌱 Stemming
- 📖 Lemmatization
- 🗑️ Stop-word handling
- #️⃣ Hashtag extraction and frequency analysis
- ⚖️ Comparing NLP libraries

These techniques are essential building blocks for more advanced NLP applications such as **sentiment analysis, text classification, information extraction, and language modeling**.

---

<div align="center">

### 🚀 Lab 2 Completed

**Faisal AL Zahrani — 2230000363**

</div>
