# Lab 9 — Named Entity Recognition (NER)

**Student Name:** Faisal AL Zahrani  
**Student ID:** 2230000363

## Overview

This Google Colab / Jupyter notebook solves the three tasks in Lab 9 using spaCy. It preserves the original lesson material and uses the required `Example.from_dict()` method for custom Named Entity Recognition training.

## Tasks

- **Task 1:** Enter an English sentence and display named entities, their labels, explanations, and character positions. Press Enter without typing to use the sample sentence.
- **Task 2:** Find and correct entities in the agreement paragraph; count `PERSON` and `ORG`, and visualize results with displaCy.
- **Task 3 (improved):** Train a custom NER pipeline using the assigned Microsoft sentence plus additional annotated examples for `ORG`, `PERSON`, `GPE`, and `PRODUCT` (including `Windows` and `Office`).

## Task 3 improvements

- Uses **10 annotated training examples** instead of only one example.
- Creates training data using **`Example.from_dict()`**.
- Trains for **exactly 20 iterations**, as required.
- Uses small mini-batches and a fixed random seed.
- Saves the model to the required **`custom_ner_model/`** directory.
- Reloads the saved model and shows predictions, a target-entity count, and an entity visualization.

**Limitations:** These toy examples are primarily for the lab exercise, not a rigorous estimate of generalization. A separate held-out evaluation dataset would be needed to measure performance on new texts.

## Run in Google Colab

1. Open https://colab.research.google.com/ and upload `Lab9_NER_Improved_Colab.ipynb`.
2. Run the cells in order (**Runtime → Run all**).
3. When Task 1 requests input, type a sentence and press Enter (or press Enter for the example).
4. Read the output under Task 3 and inspect the entity visualization. The trained model will be available under `custom_ner_model/` in the Colab session.

The first Python cell installs spaCy and downloads `en_core_web_sm` automatically if needed. Internet access is needed for initial setup.

## Submission files

```text
Lab9_NER_Improved_Submission/
├── Lab9_NER_Improved_Colab.ipynb
└── README.md
```
