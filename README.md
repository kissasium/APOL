# APOL: Learned Sentence-Level Compression for RAG

APOL-BENCH is a framework for evaluating sentence-level compression strategies in Retrieval-Augmented Generation (RAG) pipelines. The project compares heuristic retrieval-based pruning methods with a learned compression controller to study the impact of context reduction on open-domain question answering.

## Overview

We introduce the **Adaptive Hybrid Compression Controller (AHCC)**, a DistilBERT-based sentence classifier that assigns retrieved sentences into:

- **KEEP** — retain important evidence
- **DROP** — remove irrelevant context
- **EMBED** — preserve selected supporting information

The compressed context is evaluated using **Mistral-7B-Instruct** on the **HotpotQA** dataset.

## Key Results

- AHCC improves Token F1 from **0.087 → 0.181** compared to no compression (**107% improvement**).
- Reduces prompt tokens by **47.5%**.
- Maintains stable performance across retrieval sizes (**k = 10, 15, 20**).
- Reveals that classifier performance does not always translate into better RAG pipeline performance.
