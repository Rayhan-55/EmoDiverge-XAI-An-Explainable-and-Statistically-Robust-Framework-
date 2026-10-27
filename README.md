# Media–Public Emotional Divergence during the 2026 Nepal Floods

Reproducibility package for the study of **emotional divergence between news media and the public**
during the 2026 Nepal floods, using cross-model emotion labeling, Jensen–Shannon distance,
document-cluster statistical validation, explainability, topic modeling, and sentiment classification.

> **Status:** This repository is private during peer review and will be made public upon acceptance.
> Replace the placeholder URL in the paper's Data Availability Statement with this repository's link.

---

## Overview

Two parallel English corpora on the 2026 Nepal floods are compared:

- **Media** — news articles collected via the GDELT Document 2.0 API (many outlets).
- **Public** — YouTube comments on flood-related news videos (YouTube Data API v3).

Both corpora are cleaned identically, split into sentences (≥5 words), labeled by several emotion
and sentiment models, and then analyzed for (i) media–public divergence with document-cluster
uncertainty, (ii) cross-model robustness, (iii) explainability and topics, and (iv) sentiment
classification under a leakage-free, document-disjoint protocol.

---

## Repository structure

```
.
├── data/
│   ├── nepal_flood_news.csv              # Newspaper corpus (657 articles, 402 outlets)
│   ├── nepal_youtube_comments.csv        # Public corpus (3,850 English YouTube comments, anonymized)
│   └── sentence_level_labeled_data.csv   # 25,646 sentences with emotion/sentiment labels + document IDs
├── code/
│   ├── 01_divergence_analysis.ipynb      # Labeling, JS distance, bootstrap, permutation, ModernBERT, XAI, topics
│   └── 02_classification_ablation.ipynb  # Document-disjoint classification, ablation (CNN/BiLSTM/hybrid)
├── figures/                              # Figures used in the manuscript
└── README.md
```

---

## Data description

| Attribute | Newspaper (media) | YouTube (public) |
|---|---|---|
| Documents | 657 articles | 3,850 comments |
| Distinct sources | 402 outlets, 39 countries | 82 videos |
| Document clusters | 385 | 3,505 |
| Sentences (≥5 words) | 19,239 | 6,407 |
| Time span (2026) | 30 Aug – 25 Sep | 26 Aug – 29 Sep |
| Language | English | English |

**`sentence_level_labeled_data.csv`** is the main analytical artifact. Key columns:

- `source` — `newspaper` or `youtube`
- `text` — the sentence
- `date` — document timestamp
- `doc_id` — source-qualified document identifier (used for document-cluster resampling and document-disjoint splits)
- `nrc_*` — NRCLex emotion scores
- `vader`, `textblob`, `roberta_sent` — sentiment labels
- `goemotion` — GoEmotions (RoBERTa) top-one emotion label

---

## How to reproduce

1. **Environment** (Python 3.10+):
   ```bash
   pip install -r requirements.txt
   ```
2. **Run the notebooks** (Google Colab with a GPU recommended for the transformer and neural steps):
   - `code/01_divergence_analysis.ipynb` — reproduces the emotion/sentiment labeling, the
     Jensen–Shannon distance with document-cluster bootstrap and permutation tests, the
     RoBERTa↔ModernBERT agreement, the SHAP/LIME explanations, and the LDA topics.
   - `code/02_classification_ablation.ipynb` — reproduces the document-disjoint, multi-seed
     sentiment classification benchmark and the CNN / BiLSTM / hybrid ablation.

   Upload the files in `data/` (or mount them) and set the file paths in the first cells.

> **Note on terminology.** The reported quantity (`0.376` for RoBERTa GoEmotions) is the
> **Jensen–Shannon distance** (base-2); the corresponding divergence is its square. Both are reported
> consistently in the manuscript.

> **Note on evaluation.** All classification results use a **document-disjoint** split: every sentence
> from a given article or comment stays within one fold, preventing the data leakage that would
> otherwise inflate accuracy.

---

## Requirements

See `requirements.txt`. Core packages: `pandas`, `numpy`, `scipy`, `scikit-learn`, `xgboost`,
`transformers`, `torch`, `tensorflow`, `nrclex`, `vaderSentiment`, `textblob`, `gensim`,
`shap`, `lime`, `matplotlib`, `seaborn`, `wordcloud`.

---

## Ethics and data use

News text was collected from publicly indexed outlets. Public reactions are drawn from **publicly
posted** YouTube comments and are released here in **anonymized** form (no usernames or account
identifiers). Only aggregate, non-identifying analyses are reported.

---

## Citation

```
[Author(s)]. Media–Public Emotional Divergence during a Sudden-Onset Disaster:
A Multi-Model Framework Applied to the 2026 Nepal Floods. [Journal], [Year].
```

## License

Suggested: MIT License for the code, and CC BY 4.0 for the derived datasets (add `LICENSE` file before release).
