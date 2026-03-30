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

Below is a complete blueprint for your GitHub repository **`ir-ranking-evaluation-wikir-mirage`**. It includes a structured README, suggested file organisation, and instructions to reproduce all experiments from your notebook.



## 📁 Suggested Repository Structure

```
ir-ranking-evaluation-wikir-mirage/
│
├── README.md
├── requirements.txt
├── config.py                 (optional: paths, parameters)
│
├── notebooks/
│   └── HA2.ipynb             (your complete Jupyter Notebook)
│
├── scripts/
│   ├── download_novels.py    (script to fetch Tolstoy/Dostoevsky from Gutenberg)
│   └── run_all.py            (optional: run notebook cells programmatically)
│
├── data/                     (ignored by git – for local data)
│   ├── wikIR1k (1).zip
│   ├── doc_pool.json
│   ├── dataset.json
│   ├── oracle.json
│   └── novels/               (war_and_peace.txt, crime_and_punishment.txt)
│
├── runs/                     (generated TREC run files)
│   └── run_wiki_*_BM25.txt, etc.
│
├── results/                  (generated tables, plots)
│   └── wiki_results.csv
│   └── mirage_results.csv
│   └── learning_curve.png
│
└── LICENSE


```


### Download the datasets

- **WikiR (en1k)**: [https://github.com/getalp/wikiR](https://github.com/getalp/wikiR)  
  Place `wikIR1k (1).zip` inside the `data/` folder.
- **MIRAGE**: [https://github.com/nlpai-lab/MIRAGE](https://github.com/nlpai-lab/MIRAGE)  
  Place `doc_pool.json`, `dataset.json`, `oracle.json` inside `data/`.

### Run the Jupyter Notebook

```bash
jupyter notebook notebooks/HA2.ipynb
```

Execute all cells. The notebook will:
- Load the archives, preprocess texts (parallelised),
- Run TF‑IDF and BM25 on original/stemmed/lemmatised variants,
- Evaluate with `ir_measures`,
- Optimise BM25 parameters,
- Analyse MIRAGE,
- Download and compare Tolstoy & Dostoevsky paragraphs.

> **Note**: The first run may take 10–15 minutes (369k documents). Preprocessed text variants are cached in memory.



## 📂 Data Sources

| Collection | Documents | Queries | Relevance | Format |
|------------|-----------|---------|-----------|--------|
| WikiR (en1k) | 369,722 | 101 test + 101 validation | graded (1‑2) | CSV + TREC qrels |
| MIRAGE      | 7,560 passages | 7,560 | binary (oracle) | JSON |

Novels are fetched automatically from Project Gutenberg.



## 📈 Reproducibility

All runs are fully deterministic (random seeds fixed in the notebook). Intermediate files (TREC runs, cleaned data, plots) are saved locally. The notebook includes detailed output of execution times and evaluation scores.

---


## 🙏 Acknowledgements

- WikiR dataset by Frej et al. (LREC 2020)
- MIRAGE benchmark by Park et al. (NAACL 2025)
- Project Gutenberg for the novel texts


