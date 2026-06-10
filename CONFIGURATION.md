# Configuration Reference

Configuration details for the experiments reported in "Evaluating Chunking Strategies for Retrieval-Augmented Generation on Academic Texts" (ACIT 2026).

This repository contains the parameters used to produce the results in the paper. Source code is held back pending publication of the broader project; configuration is published here to support partial reproducibility.

## Hardware

| Component | Specification |
|---|---|
| GPU | 16 GiB VRAM |
| Platform | Local self-hosted, single workstation, Linux |

## Models

| Role | Model | Source |
|---|---|---|
| Embedder | `sentence-transformers/all-MiniLM-L6-v2` | Hugging Face |
| Generator LLM | `llama3.2:3b` | Ollama |
| Evaluator LLM (RAGAs judge) | `deepseek-r1:8b` | Ollama |

## Chunkers

### Fixed-Size (`WordBasedChunker`)
| Parameter | Value |
|---|---|
| Chunk size | 150 words |
| Overlap | 15 words |

### Recursive (`RecursiveWordBasedChunker`)
| Parameter | Value |
|---|---|
| Target chunk size | 150 words |
| Split hierarchy | `\n\n` → `\n` → `. ! ?` |

### Cluster-Based (`SemanticClusteringChunker`)
| Parameter | Description | Value |
|---|---|---|
| λ | Positional/semantic weight | 0.25 |
| τ | Distance threshold | 0.5 |
| `target_sents` | Sentences per chunk | 3 |
| `slack` | Max chunk size factor | 1.2 |
| Method | Clustering algorithm | single-linkage |
| Encoder | Sentence model | `all-MiniLM-L6-v2` |
| Sentence splitter | Method | regex |

## Retrieval

| Parameter | Value |
|---|---|
| Vector store | ChromaDB |
| Similarity metric | cosine |
| Top-k | 4 |

## Evaluation (RAGAs)

| Parameter | Value |
|---|---|
| RAGAs version | 0.4.3 |
| Evaluator temperature | DEFAULT: 0.8 |
| Metrics computed | faithfulness, answer_relevancy, context_precision, context_recall |
| Derived metrics | AQS = harmonic mean of faithfulness and answer_relevancy; Context F1 = harmonic mean of context_precision and context_recall |

## Dataset

| Property | Value |
|---|---|
| Documents | 13 academic theses |
| Word count range | 10,232 – 26,960 (median ≈ 16,000) |
| Format | Faculty-standardized thesis structure |
| Queries per document | 10 (5 fixed metadata, 5 thesis-specific) |
| Total measurements | 13 documents × 10 queries × 3 chunkers = 390 |

The thesis corpus is restricted and cannot be released. The QA set structure is described above; query texts are available on request.


## Reproducibility Notes

- Faithfulness scores failed in 44% of evaluation cases. Failures distribute approximately evenly across chunkers (42–47%); see paper Section IV for the methodological treatment.
- The pipeline was run once (390 measurements total). No averaging across seeds was performed.

## Citation

If you reference this configuration or build on this work, please cite the paper:

```bibtex
[TODO: add entry once camera-ready DOI is assigned]
```