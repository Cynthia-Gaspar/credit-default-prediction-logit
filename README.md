# 💳 Credit Default Prediction with Logistic Regression

## 📌 Executive Summary

A Taiwanese bank needs to identify which customers are likely to default next month. This analysis builds a logistic regression model using demographic data, payment history, and billing behavior, then tests whether performance holds across two different time periods to detect data drift.

**Result:** Model 1 achieves AUC-ROC of 0.737, correctly identifying 63% of actual defaults. The temporal split comparison reveals mild data drift, and AUC-ROC emerges as the most informative metric, accuracy is misleading given the 4:1 class imbalance.

---

## 📊 Data

| Parameter | Details |
|-----------|---------|
| **Source** | UCI ML Repository — Default of Credit Card Clients (Yeh, I.C., 2009), modified by course instructor |
| **Observations** | 30,000 credit card holders |
| **Target** | `default.payment.next.month` (1 = default, 0 = no default) |
| **Class split** | ~78% no default / ~22% default |
| **Features** | Demographics · Credit limit · Payment status (PAY_1–6) · Bill amounts · Payment amounts |

---

## 📈 Key Results

| Metric | Model 1 | Model 2 |
|--------|---------|---------|
| Accuracy | 0.7104 | 0.6575 |
| Recall (Default) | 0.63 | 0.65 |
| Precision (Default) | 0.38 | 0.36 |
| F1 (Default) | 0.47 | 0.46 |
| **AUC-ROC** | **0.7365** | **0.7182** |

Model 1 leads on four out of five metrics. The AUC-ROC gap of 0.018 is meaningful, Model 1 ranks defaulters above non-defaulters more reliably across all thresholds. Model 2's lower accuracy reflects mild data drift: training on later data does not generalize equally well to earlier observations.

**What this means for business:** Model 1 performs best as a risk-scoring mechanism to prioritize accounts for further review. For every 100 customers flagged as high-risk, 38 will actually default; this is meaningful for credit risk management but not sufficient as a standalone approve/reject tool. AUC-ROC of 0.737 means the model ranks a random defaulter above a random non-defaulter 73.7% of the time.

---

## 🛠️ Technologies

`Python` · `pandas` · `numpy` · `matplotlib` · `seaborn` · `scikit-learn`

IQR Outlier Capping · One-Hot Encoding · StandardScaler · class_weight='balanced' · AUC-ROC · Temporal Split Validation

---

## 🧭 Reflections and Learnings

- **Accuracy fails with imbalanced data.** A model predicting "no default" for everyone achieves 78% accuracy while being useless. Five metrics tell a more honest story.
- **Preprocessing decisions should be empirical.** Log1p on LIMIT_BAL improved AUC-ROC by only 0.0008 — below the 0.005 threshold considered meaningful. The simpler pipeline won.
- **Data drift is detectable through temporal splits.** The accuracy gap between models (0.71 vs 0.66) reveals that credit risk dynamics shift over time, a direct implication for model monitoring in production.
- **Cross validation has limits in temporal data.** Standard k-fold would mix data chronologically, defeating the purpose of this comparison. The right extension is Walk-Forward Validation, which preserves chronological order while improving robustness.

This work was developed as part of my Management Analytics Master's program at Queen's University.

---

## 👩‍💻 About the Author

Hi! I'm Cynthia Gaspar, a business professional pursuing a Master's degree in Management Analytics at Queen's University. I am passionate about understanding real-world problems through data. Let's connect!

🔗 [LinkedIn](https://www.linkedin.com/in/cynthia-gaspar)
🔗 [Substack](https://substack.com/@cynthiagaspar)
