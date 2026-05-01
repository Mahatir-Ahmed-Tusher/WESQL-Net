# Robust SQL Injection Detection via Weighted Ensemble of CNN, BiLSTM, Random Forest and SVM

**Cross-validated | Adversarially Tested | Production Ready**

---

## Authors
- **Mahatir Ahmed Tusher**
- Farhana Mahbuba
- Sadia Afrin
- Tushar Kanti Saha

---

## Overview

This project presents a robust and highly accurate SQL Injection (SQLi) detection system using a heterogeneous ensemble of deep learning and classical machine learning models. The framework combines **CNN**, **BiLSTM with Self-Attention**, **Random Forest**, and **SVM** through an **Out-of-Fold (OOF) weighted soft voting** mechanism.

The system utilizes a **dual-stream preprocessing** pipeline and includes a **hybrid adversarial scoring layer** to defend against sophisticated evasion attacks.

**Achieved Performance:**
- **99.65% Accuracy** on held-out test set
- **0.9996 ROC-AUC**
- **100% Adversarial Detection Rate** across 15 evasion categories
- Excellent cross-dataset generalization

---

## Key Features

- Dual-stream feature extraction (Character-level TF-IDF + Token Embeddings)
- Optuna-tuned CNN architecture
- BiLSTM with Self-Attention mechanism
- Zero data leakage through per-fold preprocessing
- Dynamic OOF-based ensemble weighting
- Hybrid (Neural + Signature) adversarial defense
- Production-ready inference pipeline

---

## Results

### 1. Held-out Test Set Performance

| Model                  | Accuracy (%) | Precision (SQLi) | Recall (SQLi) | F1-Score (SQLi) | ROC-AUC   | PR-AUC   |
|------------------------|--------------|------------------|---------------|-----------------|-----------|----------|
| **Ensemble (Proposed)**    | **99.65**    | **1.0000**       | **0.9906**    | **0.9953**      | **0.9996**| **0.9995** |
| Random Forest          | 99.76        | 1.0000           | 0.9935        | 0.9968          | 0.9997    | 0.9994   |
| SVM                    | 99.74        | 1.0000           | 0.9941        | 0.9970          | 0.9998    | 0.9996   |
| BiLSTM + Attention     | 99.59        | 1.0000           | 0.9888        | 0.9944          | 0.9994    | 0.9991   |
| CNN                    | 99.50        | 1.0000           | 0.9865        | 0.9932          | 0.9993    | 0.9990   |

### 2. Cross-Dataset Validation (SQLiV3 - 30,609 samples)

| Metric                  | Value     |
|-------------------------|-----------|
| Accuracy                | **99.80%** |
| Precision (Attack)      | 0.9999    |
| Recall (Attack)         | 0.9948    |
| F1-Score (Attack)       | 0.9973    |
| ROC-AUC                 | **0.9999** |
| PR-AUC                  | **0.9999** |

**Note**: Evaluated in zero-shot setting (no retraining, preprocessor fitted only on primary dataset).

### 3. Adversarial Robustness

**Detection Rate: 100% (15/15)** across all tested evasion techniques.

**Tested Categories:**
- Hex Encoding
- Comment Obfuscation
- Case Mixing
- URL Encoding
- String Concatenation
- Null Byte Injection
- Unicode Obfuscation
- Nested Comments
- Time-based Blind Injection
- Stacked Queries
- Boolean Blind
- Error-based Injection
- Out-of-Band Extraction
- Second-Order Injection
- AND/OR-based Tautology

---

## Architecture
![System Architecture](https://i.postimg.cc/YCYbLHpC/architecture.png)
The system follows a **dual-stream** design:
- **DL Stream**: Token embeddings → CNN & BiLSTM+Attention
- **ML Stream**: Character n-gram TF-IDF (char_wb, 1-3 grams) → RF & SVM

Final prediction is made using **OOF-weighted soft voting** + hybrid adversarial confidence floor.

## Quick Inference Demo

```python
from src.inference import SQLInjectionDetector
# Load models and preprocessor...

detector = SQLInjectionDetector(...)

queries = [
    "SELECT * FROM users WHERE id=1",
    "' OR 1=1 --",
    "'; DROP TABLE users; --",
    "SELECT name FROM products WHERE category='Books'"
]

results = detector.predict(queries)

for r in results:
    verdict = "SQL INJECTION" if r['is_malicious'] else "SAFE"
    print(f"{r['query'][:50]:<50} Score: {r['score']:.4f} → {verdict}")
```
## Datasets Used

Primary Dataset: SQL Injection Dataset (Kaggle) — 30,846 samples
External Validation: SQLiV3 / CSIC 2010 style — 30,609 samples


## Citation
Please cite this work as:
Mahatir Ahmed Tusher, Farhana Mahbuba, Sadia Afrin, Tushar Kanti Saha.
"Robust SQL Injection Detection via Weighted Ensemble of CNN, BiLSTM, Random Forest and SVM: A Cross-Validated, Adversarially Tested Framework." 2026.

## License
This project is licensed under the MIT License.

## Repository maintained by Mahatir Ahmed Tusher
For any queries or collaboration, feel free to open an issue.

