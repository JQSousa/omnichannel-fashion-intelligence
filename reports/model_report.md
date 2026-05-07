# 🛍️ Fashion Intelligence Platform — Model Report

*Auto-generated: 2026-05-07 17:11 UTC*

---

## Executive Summary

| Metric | Value |
|--------|-------|
| Total Customers Analysed | 8,000 |
| High Churn Risk (>60%) | 3,073 (38.4%) |
| VIP Customers (CLV > €400) | 2,975 (37.2%) |
| Total CLV Portfolio (12m) | €3,627,030 |
| Urgent Retention Actions | 1,938 customers |

---

## Model Performance

### Churn Prediction (XGBoost Classifier)
| Metric | Value |
|--------|-------|
| **ROC-AUC** | **0.7956** |
| PR-AUC | 0.6108 |

### CLV Prediction (XGBoost Regressor)
| Metric | Value |
|--------|-------|
| **R²** | **0.9898** |
| **MAE** | **€40.27** |
| RMSE | €52.54 |

### Segmentation (KMeans)
| Metric | Value |
|--------|-------|
| Optimal K | 2 |
| Silhouette | 0.2210 |

---

## CRM Actions
| Action | Customers | % of Base |
|--------|-----------|-----------|
| Maintenance | 2,372 | 29.6% |
| Urgent Retention | 1,938 | 24.2% |
| Reactivation | 1,884 | 23.5% |
| Nurture VIP | 1,806 | 22.6% |

---

## Top 5 Churn Drivers (SHAP)
| # | Feature | mean|SHAP| |
|---|---------|-------------|
| 1 | Days since purchase | 0.7241 |
| 2 | Support tickets | 0.3017 |
| 3 | Recency score | 0.2696 |
| 4 | Risk index | 0.2622 |
| 5 | Email open rate | 0.1200 |

---

## A/B Testing
| Campaign | Winner | Lift | p-value | Sig? |
|----------|--------|------|---------|------|
| Email vs Push | Push Notification | +30.4% | 0.0027 | ✅ |
| Discount vs Shipping | N.S. | +14.6% | 0.0801 | ⚪ |
| IG vs TikTok | TikTok Ads | +48.6% | 0.0079 | ✅ |

---
*Fashion Intelligence Platform · Senior ML Engineering Project*
