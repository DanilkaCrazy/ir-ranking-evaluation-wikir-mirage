# IR Ranking Evaluation: WikiR, MIRAGE, and Literary Similarity

[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**TF‑IDF vs BM25** with stemming/lemmatization, evaluation (P@k, MAP, nDCG), BM25 parameter optimisation, and paragraph similarity for Tolstoy & Dostoevsky.

This repository contains a complete implementation of the IR evaluation tasks from assignment **HA2**, including:

- Loading and preprocessing the WikiR (en1k) dataset (369,722 documents, 101 test/validation queries).
- Text normalisation: stemming (Porter) and lemmatisation (WordNet) with stopword removal.
- Ranking with TF‑IDF (cosine similarity) and BM25 (Okapi) on three text variants (original, stemmed, lemmatised).
- Evaluation using standard IR measures: **P@1, P@10, P@20, MAP, nDCG@20** (via `ir_measures`).
- Analysis of query difficulty, morphological processing impact, and measure correlation.
- Hyperparameter tuning for BM25 (`k1`, `b`) on a validation split to optimise nDCG@20.
- Application of the same pipeline to the **MIRAGE** passage‑retrieval benchmark (7,560 passages, binary relevance).
- Cross‑collection comparison of BM25 performance (WikiR vs MIRAGE).
- Paragraph‑level similarity search within and between Tolstoy’s *War and Peace* and Dostoevsky’s *Crime and Punishment* using TF‑IDF vectors.

All runs are saved in **TREC run format**. The code is structured as a single Jupyter Notebook and uses only open‑source libraries.

---

## 📊 Key Results (Summary)

| Collection | Model       | Variant    | MAP     | nDCG@20 | P@1  |
|------------|-------------|------------|---------|---------|------|
| WikiR      | BM25        | lemmatised | 0.1799  | 0.3651  | 0.53 |
| WikiR      | BM25        | original   | 0.1791  | 0.3624  | 0.52 |
| WikiR      | TF‑IDF      | original   | 0.1663  | 0.3522  | 0.52 |
| MIRAGE     | TF‑IDF      | original   | 0.4199  | 0.4644  | 0.34 |
| MIRAGE     | BM25        | original   | 0.4165  | 0.4562  | 0.34 |

**BM25 parameter tuning** (WikiR validation): best `k1=1.5, b=0.3` (nDCG@20=0.3147) → test set improvement from 0.3624 to 0.3630 (+0.0006).

**Paragraph similarity**: highest cross‑novel cosine similarity ≈0.36 (e.g., “peace”‑related passages). Within‑novel similarities reach 0.75.

---

