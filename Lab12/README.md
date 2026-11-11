# Lab 12: Transformers

**Student:** Faisal AL Zahrani  
**Student ID:** 2230000363

## Files

- `Lab12.ipynb` — completed notebook with saved outputs from a full run.

## What the notebook covers

The notebook loads a pretrained DistilBERT sentiment classifier, tokenizes a sentence, inspects embeddings and self-attention weights, and predicts sentiment. The final exercise loads `bert-base-uncased`, reports its layer and attention-head counts, and extracts the attention matrix from the third layer and second head for the requested sentence. A short explanation of the matrix rows and columns follows the code.

## Run it

Open the notebook in Jupyter Notebook, JupyterLab, or a compatible editor, then select **Run All**. Its first code cell installs `transformers`, `torch`, and `matplotlib` if needed. An internet connection is required the first time the pretrained models are downloaded.

## Verified results

- DistilBERT: 6 layers, 12 attention heads per layer; the example sentence is classified as **NEGATIVE**.
- BERT: 12 layers, 12 attention heads per layer, **144 total attention heads**.
- The selected BERT attention matrix has shape **11 × 11** for the 11 tokens, including `[CLS]` and `[SEP]`.
- All 6 code cells were executed successfully, with no cell errors.
