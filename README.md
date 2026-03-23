# 🛡️ Adversarial Robustness Evaluation of RoBERTa for AI Text Detection

[![Python 3.12+](https://img.shields.io/badge/python-3.12+-blue.svg)](https://www.python.org/downloads/release/python-3120/)
[![PyTorch](https://img.shields.io/badge/PyTorch-%23EE4C2C.svg?style=flat&logo=PyTorch&logoColor=white)](https://pytorch.org/)
[![Hugging Face](https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Models-orange)](https://huggingface.co/)

## 📖 Overview
This repository contains the codebase and evaluation framework for testing the adversarial robustness of Large Language Model (LLM) detectors. Specifically, it evaluates a **RoBERTa** model fine-tuned on the **HC3 (Human ChatGPT Comparison Corpus)** dataset to distinguish between human-written and AI-generated text. 

To test the model's vulnerabilities, a custom, highly-optimized implementation of the **TextFooler** adversarial attack was deployed. The framework is designed to generate meaning-preserving semantic perturbations to evaluate whether the detector relies on deep semantic understanding or superficial statistical patterns.

## 🔬 Academic Context & Methodology
This project serves as a research framework in Artificial Intelligence and Data Science, focusing on the security and reliability of AI text detectors.

* **Base Model:** `roberta-base` (Fine-tuned for sequence classification).
* **Dataset:** HC3 (Balanced samples of Human vs. ChatGPT text).
* **Attack Recipe:** A PyTorch-native adaptation of **TextFooler (Jin et al., 2019)**.
* **Constraints:** * *Semantic:* Word Embedding Cosine Similarity (threshold > 0.5) to ensure meaning is preserved.
  * *Grammatical:* NLTK-based Part-of-Speech (POS) tagging to ensure syntactic coherence (e.g., only swapping nouns for nouns).

## 🚀 Hardware & Performance Optimizations
The attack pipeline is explicitly engineered for high-performance computing environments (e.g., **NVIDIA RTX A4000 / 64GB RAM**):
* **FP16 Inference:** The model is loaded in half-precision (`torch.float16`) to maximize VRAM efficiency and accelerate synonym candidate evaluation.
* **PyTorch-Native Constraints:** To ensure cross-platform compatibility and bypass Windows `tensorflow-text` dependency loops, the Universal Sentence Encoder was replaced with a PyTorch-compatible `WordEmbeddingDistance` constraint.
* **Localized Asset Management:** NLTK corpora and Hugging Face caches are routed to local project directories (`./nltk_data/`) to prevent system-level permission errors in shared lab environments.

## 📊 Preliminary Results
The model was tested against a balanced subset of unseen HC3 data. Under strict semantic and grammatical attack constraints, the fine-tuned RoBERTa model demonstrated exceptional robustness:

| Metric | Score | Description |
| :--- | :--- | :--- |
| **Original Accuracy** | `100.0%` | Baseline accuracy on the pristine test split. |
| **Accuracy Under Attack** | `94.0%` | Accuracy maintained during active adversarial perturbation. |
| **Attack Success Rate** | `6.0%` | Percentage of texts the attacker successfully flipped (AI to Human). |
| **Avg. Perturbation** | `3.18%` | Average percentage of words altered in successful attacks. |

*Conclusion:* The model effectively resists standard semantic substitution attacks, proving it relies on deeper linguistic structures rather than brittle keywords.

## ⚙️ Installation & Setup

**1. Clone the repository:**
```bash
git clone [https://github.com/YourUsername/RoBERTa-TextFooler-Evaluation.git](https://github.com/YourUsername/RoBERTa-TextFooler-Evaluation.git)
cd RoBERTa-TextFooler-Evaluation
