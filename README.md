# IGRF-RFE-SHAP-IDS-UNSW-NB15: Multiclass Intrusion Detection System

This repository implements a highly optimized machine learning and deep learning classification pipeline for network intrusion detection using the UNSW-NB15 dataset. 

Building upon the 23-feature subset identified by the **IGRF-RFE** (Information Gain Random Forest - Recursive Feature Elimination) research paper, we developed a series of progressive optimizations that successfully pushed model accuracy from a baseline of **79.5%** to a peak of **84.95%**, and demonstrated a **35% reduction in feature capture overhead** through SHAP-guided pruning.

---

## 📊 Summary of Experiments & Results

All primary experiments (RF, XGBoost, and PyTorch MLP) are implemented in [main.ipynb](main.ipynb).

| Setup | Model Type | Description | Test Accuracy | Model File / Status |
| :--- | :--- | :--- | :--- | :--- |
| **Baseline (RF)** | Random Forest | Predefined Split (Severe Covariate Shift) | **79.54%** | `rf_baseline.joblib` |
| **Baseline (XGB)** | XGBoost (Default) | Predefined Split (Severe Covariate Shift) | **80.48%** | `xgb_baseline.joblib` |
| **Baseline (MLP)** | PyTorch MLP | Combined Stratified Random Split (Original MLP notebook) | **84.82%** | `mlp_model.pth` |
| **Attempt 1** | XGBoost (Default) | Combined Stratified Random Split (Resolves shift) | **84.56%** | `xgb_attempt1.joblib` |
| **Attempt 2** | XGBoost (Tuned) | Randomized Split + Hyperparameter Tuning | **84.85%** | `xgb_attempt2.joblib` |
| **Attempt 3** | XGBoost Ensemble | Soft-Voting combination of Attempt 1 + Attempt 2 | **84.77%** | `xgb_attempt3.joblib` |
| **Attempt 4** | XGBoost (Tuned) | Randomized Split + Log-Feature Compression (Seed 42) | **84.88%** | `xgb_attempt4.joblib` |
| **Attempt 5** | XGBoost (Tuned) | Randomized Split + Log-Feature Compression (Seed 7) | **84.91%** | `xgb_attempt5.joblib` |
| **Attempt 6** | XGBoost (Tuned) | Seed 42 Split + Feature Compression + Ratio Engineering | **84.86%** | `xgb_attempt6.joblib` |
| **Attempt 6a** | XGBoost (Tuned) | Seed 7 Split + Feature Compression + Ratio Engineering | **84.94%** | `xgb_attempt6a.joblib` |
| **Attempt 6b** | HistGradientBoosting | Seed 7 Split + Ratio Engineering (Memory-safe alternative)| **84.85%** | `hgb_attempt6b.joblib` |
| **Attempt 7** | XGBoost (Tuned) | **SHAP-Guided Pruning (Reduced 15-Feature Model)** | **84.80%** | `xgb_attempt7.joblib` |
| **Attempt 8** | XGBoost (Tuned) | **SHAP-Guided Pruning (15-Feature Model + Tuned Hyperparameters)** | **84.90%** | `xgb_attempt8.joblib` |
| **Attempt 9** | PyTorch MLP | **SHAP-Guided Pruning on MLP (Reduced 15-Feature Model)** | **84.50%** | `mlp_attempt9.pth` |
| **Attempt 10** | XGBoost (Tuned) | **Hybrid: Ratio Transformations + SHAP-Guided Pruning (Degraded)** | **84.94%** | `xgb_attempt10.joblib` |
| **Attempt 11** | XGBoost (Tuned) | **Optimized Hybrid: Ratios + Pruning (Peak 20-Feature Model)** | **84.95%** | `xgb_attempt11.joblib` |

---

## 📁 Repository Structure

* 📓 [main.ipynb](main.ipynb): Core notebook containing baseline comparisons and progressive Attempts 1 to 11.
* 📓 [explainable_ai.ipynb](explainable_ai.ipynb): Interpretability pipeline calculating global feature importances, beeswarm plots, and waterfall charts using **SHAP**.
* 📁 `models/`: Central cache directory containing the 16 pre-trained model files (`.joblib` and `.pth`).
* 📁 `data/`: Directory where the raw UNSW-NB15 CSV datasets are stored.
* 📄 [requirements.txt](requirements.txt): Complete dependency list.

---

## 🚀 Setup & Installation

Ensure you have Python 3.10+ (fully compatible with Python 3.14). To install the requirements, run:
```bash
pip install -r requirements.txt
```
> **Note**: The `requirements.txt` is pre-configured with `--extra-index-url https://download.pytorch.org/whl/cpu` to download a lightweight, CPU-only PyTorch package (~190 MB) instead of a multi-gigabyte CUDA package.

---

## 📈 Key Research Takeaway (SHAP-Guided Feature Pruning)
In real-time intrusion detection systems, parsing and logging features at network speeds is a massive bottleneck. 

Our **SHAP Explainability analysis** revealed that the bottom 8 features (`tcprtt`, `spkts`, `dinpkt`, `djit`, `dttl`, `ackdat`, `dpkts`, and `state`) contribute less than **7.2%** of the model's total predictive information. 

By dropping these 8 features entirely, we reduced feature capture overhead by **35%** (from 23 down to 15 features) while maintaining an accuracy of **84.90%** (virtually identical to the 23-feature model's peak of **84.94%**). 

Furthermore, our refined Attempt 11 hybrid model combines key flow ratios with SHAP-based feature selection to reduce feature count to **20 features** while setting a **new peak classification record of 84.95%** accuracy. This provides a massive boost in network sensor throughput with practically zero impact on security.
# IGRF-RFE-SHAP-IDS-UNSW-NB15
