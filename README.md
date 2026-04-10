# GPT-2 Biomedical Fine-Tuning

This project was completed as an assignment for **CMPE454 — Large Language Models** course. The goal was to demonstrate staged transfer learning by transforming a general-purpose GPT-2 Small model into a biomedical domain specialist capable of following instructions.

## Overview

Rather than training from scratch, the project fine-tunes GPT-2 through four distinct phases — each building on the previous one.

## Phases

### Phase 1 — Baseline Evaluation
Evaluated the pre-trained GPT-2 Small model on a biomedical corpus (PubMed abstracts) to establish a baseline.
- **Perplexity: 31.86**

### Phase 2 — Domain Adaptation
Fine-tuned GPT-2 on the biomedical corpus using Causal Language Modeling (CLM). The model learns the language patterns and terminology of the medical domain.
- **Perplexity dropped to: 16.80**

### Phase 3 — Discriminator
Repurposed the domain-adapted model as a binary classifier to distinguish biomedical text from general content. Trained on a balanced dataset of biomedical abstracts and synthetic general-domain sentences.
- **Test Accuracy: 100%**

### Phase 4 — Instruction Fine-Tuning
Fine-tuned the domain-adapted model on 50 curated instruction-response pairs using the `### Instruction / ### Response` format, turning it into a biomedical assistant.

**Example:**
```
### Instruction:
What is Cox proportional hazards regression?

### Response:
Cox proportional hazard regression measures the influence of a given outcome
on the likelihood that it will occur...
```

## Files

| File | Description |
|------|-------------|
| `cmpe454_p1.ipynb` | Phase 1 — Baseline evaluation |
| `cmpe454_p2.ipynb` | Phase 2 — Domain adaptation |
| `cmpe454_p3.ipynb` | Phase 3 — Binary classifier |
| `cmpe454_p4.ipynb` | Phase 4 — Instruction fine-tuning |
| `dataset_454.ipynb` | Dataset preparation |
| `instruct.json` | 50 instruction-response pairs |
| `classification.csv` | Labeled dataset for Phase 3 |

## Setup

```bash
pip install torch transformers datasets evaluate matplotlib
```

Run the notebooks in order: `p1 → p2 → p3 → p4`

> Note: Phase 2 saves the domain-adapted model to `./gpt2domain`, which is required by Phases 3 and 4. Phase 4 saves the final model to `./gpt2-instruct-final`.

## Limitations

**Phase 3 — 100% accuracy caveat:** The classifier achieved perfect accuracy, but this should be interpreted carefully. The negative class consisted of synthetically generated sentences (e.g. *"John walked to the store..."*) — artificially simple and structurally nothing like real text. The separation between the two classes was trivially easy. If evaluated against real-world non-biomedical text (news articles, Wikipedia, etc.), accuracy would likely be lower. A more meaningful evaluation would use real general-domain text as negatives.

**Phase 4 — Small instruction set:** The instruction fine-tuning was done on only 50 pairs, which is a very small dataset. The model's responses are reasonable but limited in variety and depth.

## Tech

- [Hugging Face Transformers](https://huggingface.co/docs/transformers)
- [GPT-2 Small](https://huggingface.co/gpt2)
- PyTorch
