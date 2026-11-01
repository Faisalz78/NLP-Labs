# Lab 8: Part-of-Speech (POS) Tagging

This repository contains the completed **Lab 8: Part-of-Speech (POS) Tagging** notebook for a Natural Language Processing (NLP) lab. The exercises demonstrate how to identify the grammatical role of words using **NLTK** and **spaCy**.

## Learning Objectives

- Understand how Part-of-Speech (POS) tagging works.
- Use **NLTK** to tokenize text and assign POS tags.
- Use **spaCy** to extract coarse-grained (`pos_`) and fine-grained (`tag_`) tags.
- Understand how the same word can have different grammatical roles depending on context.
- Visualize dependency relationships between words using **displaCy**.

## Lab Tasks

| Task | Description |
| --- | --- |
| **Task 1** | Identify the POS and fine-grained POS tags of **“play”**, explain the tag, and print the tags of every word in the sentence. |
| **Task 2** | Compare the grammatical roles of **“Google/google”** and **“table”** in different sentences. |
| **Task 3** | Generate a dependency visualization for: *“I like to play football. I hated it in my childhood though”*. |

## Files

- `Lab8_POS_Solved_Colab.ipynb` — Completed Jupyter Notebook with explanations, examples, and solutions to all three tasks.

## How to Run on Google Colab

1. Open [Google Colab](https://colab.research.google.com/).
2. Select **File → Upload notebook**.
3. Upload `Lab8_POS_Solved_Colab.ipynb`.
4. Select **Runtime → Run all**.
5. Review the POS-tagging outputs and the dependency visualization.

The notebook downloads the required **NLTK** data and attempts to install the spaCy English model (`en_core_web_sm`) when it is missing.

## Requirements

- Python 3
- NLTK
- spaCy
- spaCy English model: `en_core_web_sm`
- Jupyter Notebook or Google Colab

If running locally, install the dependencies with:

```bash
pip install nltk spacy notebook
python -m spacy download en_core_web_sm
```

## Example

The same word may have different POS tags based on its role in a sentence:

- **“Did you search on Google?”** — *Google* is a proper noun.
- **“Let us google it.”** — *google* is a verb.
- **“The food is on the table.”** — *table* is a noun.
- **“She plans to table a motion.”** — *table* is a verb.

## Notes

POS tags are predictions made by language-processing tools. Exact tags can vary with the model version and sentence context.

---

**Course topic:** Natural Language Processing (NLP) · **Lab:** 8 — Part-of-Speech (POS)
