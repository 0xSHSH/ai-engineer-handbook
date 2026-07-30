# 4️⃣ Machine Learning Interview

> Part of the [Interview Handbook](README.md). Classical ML: algorithms, evaluation, and the questions that separate "used sklearn once" from "understands the model."

## 📑 Contents
- [Core Algorithms](#core-algorithms)
- [Ensemble Methods](#ensemble-methods)
- [Feature Engineering & Data Cleaning](#feature-engineering--data-cleaning)
- [Model Evaluation](#model-evaluation)
- [Bias vs Variance](#bias-vs-variance)
- [Hyperparameter Tuning](#hyperparameter-tuning)
- [Interview & Coding Questions](#interview--coding-questions)

---

## Core Algorithms

| Algorithm | Type | Key idea | Watch out for |
|---|---|---|---|
| **Linear Regression** | Regression | Fits `y = wx + b` minimizing squared error (closed-form or gradient descent) | Sensitive to outliers, assumes linear relationship, multicollinearity inflates coefficient variance |
| **Logistic Regression** | Classification | Applies sigmoid to a linear combination, optimizes log-loss | Not "logistic" as in curve-fitting — it's a linear *classifier* in log-odds space |
| **Decision Trees** | Both | Recursive splits maximizing information gain / Gini reduction | Prone to overfitting; unstable to small data changes |
| **Random Forest** | Both | Bagging of decision trees + random feature subsets | Reduces variance vs a single tree; less interpretable |
| **SVM** | Classification | Maximizes margin between classes; kernel trick for non-linear boundaries | Doesn't scale well past ~100k rows without approximation |
| **Naive Bayes** | Classification | Applies Bayes' theorem assuming feature independence | The "naive" independence assumption is usually false but works surprisingly well for text |
| **KNN** | Both | Predicts from the k nearest points by distance | No training phase, but expensive/slow at inference; needs feature scaling |
| **Gradient Boosting** | Both | Sequentially fits trees to residual errors | Powerful but easy to overfit without regularization/early stopping |

### Bias-variance intuition per model
- **High bias (underfit):** Linear regression on non-linear data, shallow decision tree, high-regularization models.
- **High variance (overfit):** Deep unpruned decision tree, KNN with k=1, high-degree polynomial regression.

---

## Ensemble Methods

| Method | Mechanism | Library |
|---|---|---|
| **Bagging** (Random Forest) | Train many models on bootstrapped samples in parallel, average/vote | Reduces variance |
| **Boosting** (AdaBoost, GBM) | Train models sequentially, each correcting the previous one's errors | Reduces bias, risk of overfitting if unchecked |
| **XGBoost** | Gradient boosting + regularization (L1/L2), tree pruning, parallel split-finding | Industry default for tabular data for years |
| **LightGBM** | Histogram-based splitting, leaf-wise growth (vs level-wise) | Faster on large datasets, can overfit on small ones |
| **CatBoost** | Ordered boosting + native categorical feature handling | Less preprocessing needed for categorical-heavy data |

**Interview one-liner:** "Bagging reduces variance by averaging independent models; boosting reduces bias by chaining models that fix each other's mistakes."

---

## Feature Engineering & Data Cleaning
- **Missing values**: drop (if <5% and MCAR), mean/median/mode impute, or model-based impute (KNN/iterative imputer). Always ask *why* it's missing — MCAR vs MAR vs MNAR changes the right strategy.
- **Categorical encoding**: one-hot (low cardinality), target/mean encoding (high cardinality, watch for leakage — encode using only training fold), ordinal (when order is meaningful).
- **Scaling**: standardization (z-score) for distance-based/gradient-based models (SVM, KNN, neural nets); tree-based models are scale-invariant and don't need it.
- **Outliers**: IQR method, z-score threshold, or domain-driven caps — don't blindly drop without checking if they're signal (e.g., fraud).
- **Leakage**: the #1 real-world bug. Any transformation using statistics from the full dataset (mean, scaler, target encoding) must be *fit on train only*, then applied to val/test.

## Model Evaluation

| Metric | Use when | Formula/idea |
|---|---|---|
| Accuracy | Balanced classes | (TP+TN)/Total |
| Precision | False positives are costly | TP/(TP+FP) |
| Recall | False negatives are costly | TP/(TP+FN) |
| F1 | Need a balance of precision/recall | 2·P·R/(P+R) |
| ROC-AUC | Ranking quality across thresholds | Area under TPR vs FPR |
| PR-AUC | Imbalanced classes | Area under precision vs recall — more informative than ROC-AUC when positives are rare |
| RMSE / MAE | Regression | RMSE penalizes large errors more (squared); MAE is robust to outliers |

**Cross-validation**: k-fold CV (typically k=5 or 10) gives a more reliable estimate of generalization than a single train/test split. Use **stratified k-fold** for classification with imbalanced classes, and **time-series split** (no shuffling) for temporal data — shuffling time series causes leakage from the future.

## Bias vs Variance
- **Bias**: error from overly simplistic assumptions (underfitting) — high train AND test error.
- **Variance**: error from sensitivity to training data noise (overfitting) — low train error, high test error.
- **Total error = Bias² + Variance + Irreducible error.**
- Fixes for high bias: more complex model, more features, less regularization.
- Fixes for high variance: more data, regularization (L1/L2), simpler model, ensembling, early stopping.

## Hyperparameter Tuning
- **Grid search**: exhaustive, expensive, fine for small search spaces.
- **Random search**: often finds good configs faster than grid search for high-dimensional spaces.
- **Bayesian optimization** (Optuna, Hyperopt): models the objective function to pick promising points — most sample-efficient for expensive-to-train models.
- Always tune on a validation set (or via CV), never the test set — test set is for the final, single reported number.

---

## Interview & Coding Questions

**Conceptual**
1. Explain the bias-variance tradeoff with an example.
2. Why does Random Forest reduce overfitting compared to a single decision tree?
3. When would you choose PR-AUC over ROC-AUC?
4. Why is accuracy a bad metric for fraud detection?
5. How does XGBoost differ from a plain gradient boosting machine?

**Coding**
```python
# Implement k-fold cross-validation from scratch (conceptual skeleton)
import numpy as np

def k_fold_indices(n, k=5, seed=42):
    rng = np.random.default_rng(seed)
    idx = rng.permutation(n)
    folds = np.array_split(idx, k)
    for i in range(k):
        val_idx = folds[i]
        train_idx = np.concatenate([folds[j] for j in range(k) if j != i])
        yield train_idx, val_idx
```
```python
# Implement precision/recall/F1 from raw predictions
def precision_recall_f1(y_true, y_pred):
    tp = sum(1 for t, p in zip(y_true, y_pred) if t == 1 and p == 1)
    fp = sum(1 for t, p in zip(y_true, y_pred) if t == 0 and p == 1)
    fn = sum(1 for t, p in zip(y_true, y_pred) if t == 1 and p == 0)
    precision = tp / (tp + fp) if (tp + fp) else 0.0
    recall = tp / (tp + fn) if (tp + fn) else 0.0
    f1 = 2 * precision * recall / (precision + recall) if (precision + recall) else 0.0
    return precision, recall, f1
```

---
*Part of the [AI Engineer Handbook](../../README.md) · [Interview Handbook](README.md) · Next: [Deep Learning](05_dl_interview.md).*
