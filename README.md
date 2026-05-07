# 🛍️ Omnichannel Customer Intelligence Platform

> **Senior ML Engineering Project** — Fashion & Retail Analytics  
> End-to-end machine learning pipeline: churn prediction, CLV forecasting, customer segmentation, A/B testing, and explainability (XAI) with a full MLOps workflow.

---

[![Python](https://img.shields.io/badge/Python-3.10-blue?style=flat-square&logo=python)](https://python.org)
[![XGBoost](https://img.shields.io/badge/XGBoost-2.0-orange?style=flat-square)](https://xgboost.readthedocs.io)
[![SHAP](https://img.shields.io/badge/SHAP-0.44-purple?style=flat-square)](https://shap.readthedocs.io)
[![MLflow](https://img.shields.io/badge/MLflow-2.10-blue?style=flat-square&logo=mlflow)](https://mlflow.org)
[![CI/CD](https://img.shields.io/badge/CI%2FCD-GitHub%20Actions-2088FF?style=flat-square&logo=githubactions)](https://github.com/features/actions)
[![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)

---

## 📋 Overview

This project demonstrates a **production-grade ML pipeline** for a mid-market fashion retailer operating across multiple channels (e-commerce, mobile app, physical stores, social commerce). The platform ingests raw omnichannel data, engineers features through a lakehouse architecture (Bronze → Silver → Gold), trains three ML models, explains every prediction with SHAP, validates campaigns with statistical A/B tests, and surfaces insights in an executive dashboard — all tracked by MLflow and gated by a 7-step CI/CD pipeline.

### Business Problems Solved

| Problem | Model | Business Impact |
|---------|-------|----------------|
| Which customers will leave in the next 90 days? | **Churn Prediction** (XGBoost Classifier) | Proactive retention saves 30-60% of churning revenue |
| How much will each customer spend next year? | **CLV Forecast** (XGBoost Regressor) | Prioritizes retention budget by expected return |
| How do we personalise at scale? | **Segmentation** (KMeans) | Tailored campaigns per behavioral cluster |
| Which campaign variant actually works? | **A/B Testing** (Z-test) | Statistically validated marketing decisions |

---

## 🏗️ Architecture

```
Raw Sources (omnichannel)
         │
         ▼
┌─────────────────────────────────────────┐
│  🥉  BRONZE LAYER                       │
│  customers_raw.csv                      │
│  transactions_raw.csv                   │
│  8,000 customers · 35,000 transactions  │
└─────────────────┬───────────────────────┘
                  │  dedup · join · label encoding
                  │  9 derived RFM features
                  ▼
┌─────────────────────────────────────────┐
│  🥈  SILVER LAYER                       │
│  unified_customer_profile.csv           │
│  8,000 rows × 45 columns               │
└─────────────────┬───────────────────────┘
                  │  CLV target engineering
                  │  feature selection
                  ▼
┌─────────────────────────────────────────┐
│  🥇  GOLD LAYER                         │
│  churn_features.csv                     │
│  clv_features.csv                       │
│  25 ML-ready features per model         │
└──────┬──────────────┬───────────────────┘
       │              │              │
       ▼              ▼              ▼
┌────────────┐ ┌────────────┐ ┌────────────┐
│   CHURN    │ │    CLV     │ │    SEG     │
│  XGBoost   │ │  XGBoost   │ │   KMeans   │
│ Classifier │ │ Regressor  │ │            │
│ AUC=0.795  │ │  R²=0.990  │ │  K=2 opt  │
└─────┬──────┘ └─────┬──────┘ └─────┬──────┘
      │              │              │
      └──────────────┴──────────────┘
                     │
                     ▼
     SHAP XAI · A/B Testing · Dashboard · MLflow
```
---

## 📊 Model Performance

| Model | Algorithm | Key Metric | Value |
|-------|-----------|-----------|-------|
| Churn Prediction | XGBClassifier | **ROC-AUC** | **0.7953** |
| CLV Forecast | XGBRegressor | **R²** | **0.9897** |
| CLV Forecast | XGBRegressor | **MAE** | **€41.38** |
| Segmentation | KMeans | **Silhouette** | **0.2289** |

---

## ✨ Key Features

### 🤖 Machine Learning
- **Churn Prediction** — XGBoost with `scale_pos_weight` to handle class imbalance; calibrated probability scores for CRM integration
- **CLV Regression** — 12-month customer lifetime value with business-interpretable MAE in €
- **Segmentation** — KMeans with automated K selection via Silhouette Score; behavioral cluster naming heuristic

### 🔍 Explainability (XAI)
- **Global SHAP** — feature importance bar + beeswarm plot (direction and magnitude of each feature)
- **Dependence Plot** — functional relationship between top feature value and SHAP impact
- **Waterfall Plot** — additive decomposition for the highest-risk individual customer
- **CLV SHAP** — importance in € units (business-native scale)

### 🧪 A/B Testing
- Z-test of proportions (two-sided), pooled standard error
- Uplift curve — incremental ROI by targeting the most responsive customers
- Forest plot — simultaneous 95% CI comparison across multiple experiments

### 📊 Executive Dashboard
- KPI cards: total customers, high-risk count, average CLV, total portfolio value
- CRM Action Matrix: 4 quadrants (Churn × CLV) → Urgent Retention / Nurture VIP / Reactivation / Maintenance
- Portfolio scatter: identify "golden clients" at risk (high CLV + high churn probability)

### 📦 MLOps
- **MLflow** experiment tracking for all 3 models (params, metrics, artifacts)
- **GitHub Actions** 7-step CI/CD pipeline
- **joblib** serialization for fast inference
- Automated markdown report generation post-retraining

---

## 📁 Project Structure

```
omnichannel-fashion-intelligence/
│
├── .github/
│   └── workflows/
│       └── ml_pipeline.yml              # 7-step CI/CD pipeline
│
├── notebooks/
│   └── Fashion_Intelligence_Platform.ipynb  # Google Colab (41 cells)
│
├── src/
│   ├── validation.py                    # Data generation + schema/null/leakage checks
│   ├── train.py                         # Train all 3 models with MLflow tracking
│   ├── predict.py                       # Batch scoring pipeline
│   ├── monitoring.py                    # Drift detection + report generation
│   ├── mlflow_registry.py               # Model registry promotion logic
│   └── tests/
│       └── test_pipeline.py             # 15+ pytest test cases
│
├── requirements.txt
└── README.md
```
---

## 🚀 Quick Start

### Option 1 — Google Colab (recommended)
Open `notebooks/Fashion_Intelligence_Platform.ipynb` in [Google Colab](https://colab.research.google.com). The notebook is self-contained: it installs dependencies, generates synthetic data, trains all models, and produces every visualization.

### Option 2 — Local

```bash
git clone https://github.com/JQSousa/omnichannel-fashion-intelligence.git
cd omnichannel-fashion-intelligence
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
python src/validation.py
python src/train.py
python src/predict.py
python src/monitoring.py
mlflow ui --backend-store-uri mlruns/
```

---

## 📐 Feature Engineering — 9 Derived Features (Silver Layer)

| Feature | Formula | Business Meaning |
|---------|---------|-----------------|
| `recency_score` | `1 / (days_since_purchase + 1)` | Normalizes recency to continuous scale |
| `frequency_score` | `log(1 + total_purchases)` | Compresses skewed purchase distribution |
| `monetary_score` | `log(1 + total_revenue)` | Compresses skewed revenue distribution |
| `revenue_per_purchase` | `total_revenue / (purchases + 1)` | Stabilized average order value |
| `engagement_index` | `0.3·email + 0.4·(sessions/60) + 0.3·omni` | Composite engagement score |
| `risk_index` | `Σ binary risk signals × weights` | Aggregated churn signal (0-8 scale) |
| `channel_diversity` | `n unique channels` | Breadth of omnichannel behavior |
| `discount_sensitivity` | `mean(discount_applied)` | Propensity to buy on promotion |
| `brand_loyalty_index` | `1 / (n_brands + 1)` | Inverse of brand diversification |

---

## ⚙️ CI/CD Pipeline
Step 1 → Code Quality      flake8 · black · isort
Step 2 → Automated Tests   pytest (15+ test cases)
Step 3 → Data Validation   schema · null checks · leakage detection
Step 4 → Model Retraining  XGBoost Classifier + Regressor + KMeans
Step 5 → MLflow Registry   promote best model to Production
Step 6 → Batch Scoring     score all customers, export CSV
Step 7 → Report Generation markdown report + PNG visualizations

---

## 🧪 A/B Testing Results

| Experiment | Control | Variant | Lift | p-value | Result |
|-----------|---------|---------|------|---------|--------|
| Email vs Push Notification | 8.2% | 11.3% | **+37.8%** | < 0.05 | ✅ Push wins |
| Discount 10% vs Free Shipping | 14.5% | 17.8% | **+22.8%** | < 0.05 | ✅ Free Shipping wins |
| Instagram vs TikTok Ads | 3.1% | 2.8% | −9.7% | > 0.05 | ⚪ Inconclusive |

---

## 🔍 Top Churn Drivers (SHAP)

| Rank | Feature | Interpretation |
|------|---------|---------------|
| 1 | Days since last purchase | Primary signal — the longer the silence, the higher the risk |
| 2 | Risk index | Aggregated composite of all risk signals |
| 3 | Engagement index | Low email/app engagement predicts disengagement |
| 4 | Recency score | Inverse of recency — complement of #1 |
| 5 | NPS score | Unhappy customers are 3× more likely to churn |

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Language | Python 3.10 |
| ML Framework | XGBoost 2.0, scikit-learn 1.4 |
| Explainability | SHAP 0.44 (TreeExplainer) |
| MLOps | MLflow 2.10 |
| Visualization | Matplotlib (dark fashion theme) |
| Statistics | SciPy (Z-test, binned statistics) |
| CI/CD | GitHub Actions |
| Notebook | Google Colab |

---

## 📄 License

MIT — see [LICENSE](LICENSE) for details.

---
