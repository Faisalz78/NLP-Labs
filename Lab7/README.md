# Lab 7 — Large Language Models (LLMs)

**Course:** Natural Language Processing (NLP)  
**Student:** Faisal AL Zahrani  
**Student ID:** 2230000363

## Overview

The completed [Lab7.ipynb](Lab7.ipynb) demonstrates the three Transformer families using DistilGPT-2, FLAN-T5-small, and a DistilBERT sentiment classifier. It includes all three worked examples, every task supplied in the source, the optional challenge, and actual saved outputs.

All **10 code cells** executed successfully on Python 3.12.14 with CPU inference. Execution was verified with Hugging Face and Transformers offline modes enabled after downloading the models. No API key or paid inference service is used.

## Files

```text
Lab7/
├── Lab7.ipynb
├── README.md
├── requirements.txt
├── models_manifest.json
├── .gitattributes
├── .gitignore
└── models/                 # downloaded locally; excluded from Git and the solution ZIP
    ├── distilgpt2/
    ├── flan-t5-small/
    └── distilbert-base-uncased-finetuned-sst-2-english/
```

The model files are already present locally under `Lab7/models/` and occupy approximately **934.9 MB**. The solution ZIP contains the notebook, README, dependency list, model manifest, and Git configuration, **without the large model weights**. On another computer, running the notebook downloads missing models automatically. No separate dataset is needed: all input text is included in the notebook.

## Completed Requirements

| Section | Completed work |
|---|---|
| Part A — Decoder-only | Generate from the exact prompt “Natural language processing is” with DistilGPT-2, up to 30 new tokens, and greedy decoding. |
| Part B — Encoder–decoder | Translate the original English sentence with FLAN-T5-small, preserving the model's actual output and adding a clearly marked human reference. |
| Part C — Encoder-only | Classify the source sentence about the NLP lab with the DistilBERT SST-2 sentiment model. |
| Task 1 | Choose the conventional model family for all five listed NLP tasks and explain each choice. |
| Task 2 | Print sentiment labels and scores for the three exact sentences. |
| Task 3 | Summarize an original short paragraph with FLAN-T5-small and review the result. |
| Task 5 | Answer the chatbot-architecture reflection in exactly four sentences. |
| Optional challenge | Generate three sampled continuations without resetting a seed, explain variation, and compare with seeded and greedy controls. |

**There is no Task 4 in the supplied notebook.** The original numbering and all 19 original cells are retained in order; no supplied task was skipped. All original Markdown instructions are preserved verbatim.

## Task 1 — Model Families

| Task | Conventional best fit |
|---|---|
| Sentiment classification | Encoder-only |
| Machine translation | Encoder–decoder |
| Open-ended text generation | Decoder-only |
| Named Entity Recognition | Encoder-only |
| Text summarization | Encoder–decoder |

These are common architectural choices, not exclusive capabilities. The notebook includes examples and reasons for every choice.

## Saved Results

### Sentiment Classification

| Sentence | Predicted label | Model score |
|---|---|---:|
| The movie was excellent. | POSITIVE | 0.999861 |
| The lecture was difficult to understand. | NEGATIVE | 0.999665 |
| I enjoyed learning about transformers. | POSITIVE | 0.999746 |

The source example, “This NLP lab is easy and interesting.”, is classified as **POSITIVE**, with score **0.999779**. These scores are the classifier's softmax values, not guarantees of correctness or benchmark accuracy.

### Summarization

The original **49-word** paragraph describes a new university-library digital learning center, its equipment and workshops, and its accessibility goals. The model generated this **10-word** summary:

> The university library has opened a new digital learning center.

It captures the central event and omits supporting details. This is a qualitative assessment of one example, not a general summarization score.

### Translation

The exact source prompt is `Translate to French: I love natural language processing.` The small model returned:

> Je s'agissant de la technologie de linguage naturelle.

This output is grammatically unreliable and does not faithfully preserve “I love.” A **human-written reference**, separate from the model output, is:

> J'aime le traitement automatique du langage naturel.

The notebook preserves the actual generated translation and explains this limitation instead of presenting the reference as a model prediction.

### Generation and Randomness

The optional challenge produced **3 distinct outputs in three calls** in the saved run. Each call used the same prompt, `max_new_tokens=30`, `do_sample=True`, `temperature=0.8`, `top_k=50`, and `top_p=0.92`, without a seed reset in the sampling cell.

Two additional calls that each reset `set_seed(42)` produced identical sampled text in the tested environment. Repeating the greedy call also produced an identical result. Sampling draws tokens probabilistically, so repeated calls can vary even when the prompt is unchanged; outputs are not guaranteed to differ every time.

DistilGPT-2 can repeat text, generate unsupported statements, or stop mid-sentence at the token limit. It is a small continuation model rather than an instruction-tuned chatbot. Only trailing blank lines are trimmed for display; the returned strings remain available in the notebook variables.

## Run on Windows

Use **Python 3.12** with the pinned dependencies. With Python 3.12 installed:

```powershell
cd "C:\Users\faisa\Documents\GitHub\NLP-Labs\Lab7"
py -3.12 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m ipykernel install --user --name nlp-lab7 --display-name "Python 3.12 (NLP Lab7)"
.\.venv\Scripts\python.exe -m jupyterlab
```

Open `Lab7.ipynb`, select **Python 3.12 (NLP Lab7)**, and choose **Restart Kernel and Run All Cells**. VS Code can use the same environment. Keep the working directory set to `Lab7` so that the notebook finds the existing `models/` directory.

The setup checks dependencies instead of using the source's unpinned shell install. If complete local model files and revision markers exist, they are loaded directly. If files are missing, the notebook downloads the pinned public revisions. After dependencies and models are installed, execution works offline. Saved outputs can be reviewed without running any cells.

For another computer, either let the notebook download the models or copy the existing `models/` directory, including each `lab_revision.json` marker, alongside the notebook. The large model directories, cache, and virtual environment are ignored by Git. They are not part of the solution ZIP.

## Model Versions and Verification

| Model repository | Pinned revision |
|---|---|
| distilbert/distilgpt2 | `2290a62682d06624634c1f46a6ad5be0f47f38aa` |
| google/flan-t5-small | `0fc9ddf78a1e988dac52e2dac162b0ede4fd74ab` |
| distilbert/distilbert-base-uncased-finetuned-sst-2-english | `714eb0fa89d2f80546fda750413ed43d93601a13` |

The canonical `distilbert/` repository names refer to the same DistilGPT-2 and DistilBERT checkpoints named in the lab. The included [models_manifest.json](models_manifest.json) records source repositories, revisions, file sizes, SHA-256 fingerprints, and model-card license declarations. All three model cards declare **Apache-2.0**. Original model-card files remain in the local model directories.

Weights are loaded from Safetensors files with remote model code disabled. The experiment uses CPU inference and fixed package/model versions; numerical values and sampled outputs can still differ with other versions or execution environments. The optional unseeded examples are intentionally variable.

Verification covered notebook format, execution of all ten code cells without errors, exact preservation of source instructions and task sentences, the four-sentence reflection, all three downloaded models, seed/greedy consistency, and file checksums. The saved notebook contains no validation-only probe cell or machine-specific runtime paths.

## References

- [DistilGPT-2 model card](https://huggingface.co/distilbert/distilgpt2)
- [FLAN-T5-small model card](https://huggingface.co/google/flan-t5-small)
- [DistilBERT SST-2 sentiment model card](https://huggingface.co/distilbert/distilbert-base-uncased-finetuned-sst-2-english)
- [Transformers generation options](https://huggingface.co/docs/transformers/v4.57.1/en/main_classes/text_generation)
