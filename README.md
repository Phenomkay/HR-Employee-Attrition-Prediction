# HR Employee Attrition Prediction Project

## Project Overview

This project focuses on predicting **employee attrition** using supervised machine learning techniques. The objective is not just to achieve high predictive performance, but to build an **interpretable, leakage-free, and deployable** model that can help HR teams proactively identify employees at risk of leaving and take data-driven retention actions.

The workflow follows an end-to-end data science pipeline:

* Data understanding and cleaning
* Exploratory Data Analysis (EDA)
* Feature engineering and categorical analysis
* Handling data leakage risks
* Model training and comparison
* Threshold optimization
* Model interpretation using SHAP
* Model persistence for future deployment

---

## 1. Data Understanding

The dataset contains employee-level information covering:

* **Demographics** (age, gender, marital status)
* **Job-related factors** (department, role, job level, job satisfaction)
* **Compensation** (monthly income, stock options)
* **Work conditions** (overtime, work-life balance, years at company)
* **Target variable**: `Attrition` (Yes / No)

The target variable is **binary and imbalanced**, with fewer employees leaving than staying.

---

## 2. Data Cleaning

Key cleaning steps included:

### 2.1 Target Encoding

* Converted `Attrition` from categorical (`Yes` / `No`) to binary (`1` / `0`).

### 2.2 Removing Non-Informative Features

* Columns with **constant values** (e.g., employee count fields) were dropped.
* Identifier-like columns (e.g., EmployeeNumber) were excluded to avoid memorization.

### 2.3 Data Type Validation

* Ensured numerical features were correctly typed as numeric.
* Categorical features were explicitly cast to `object` type.

---

## 3. Exploratory Data Analysis (EDA)

EDA was conducted to understand both **global patterns** and **category-level drivers of attrition**.

### 3.1 Overall Attrition Distribution

* Attrition rate was confirmed to be **imbalanced**, motivating careful metric selection beyond accuracy.

### 3.2 Categorical Feature Analysis

Attrition rates were analyzed across key categorical variables:

* **Department** – Certain departments showed consistently higher attrition.
* **Job Role** – Specific roles exhibited elevated churn risk.
* **Overtime** – Employees working overtime had significantly higher attrition.
* **Marital Status** – Single employees showed higher attrition tendencies.

Bar plots and normalized percentages were used to ensure fair comparisons across groups.

### 3.3 Numerical Feature Analysis

Key numerical insights:

* Lower **monthly income** correlated with higher attrition.
* Employees with fewer **years at company** were more likely to leave.
* Job satisfaction and environment satisfaction showed inverse relationships with attrition.

Boxplots and distribution plots were used to compare attrition vs non-attrition groups.

---

## 4. Feature Engineering

### 4.1 Categorical Encoding

* One-Hot Encoding was applied to nominal categorical features.
* Encoding was performed **inside pipelines** to avoid data leakage.

### 4.2 Scaling

* Numerical features were scaled where required (especially for Logistic Regression).
* Scaling was also handled inside pipelines to preserve train–test separation.

---

## 5. Train–Test Split & Leakage Prevention

To prevent data leakage:

* The dataset was split into **training and test sets before any fitting**.
* All preprocessing steps (encoding, scaling) were learned **only from the training data**.
* Pipelines ensured transformations were consistently applied during inference.

This approach eliminated common leakage sources such as:

* Target-informed preprocessing
* Dataset-wide scaling or encoding

---

## 6. Model Training

Three models were trained and compared:

### 6.1 Logistic Regression

* Served as a strong, interpretable baseline.
* Enabled direct probability outputs and coefficient analysis.

### 6.2 Random Forest Classifier

* Captured non-linear relationships and feature interactions.
* Provided robust performance without heavy preprocessing.

### 6.3 XGBoost Classifier

* Delivered the strongest overall performance.
* Particularly effective at handling imbalance and complex patterns.

All models were trained using **pipelines** combining preprocessing and estimation.

---

## 7. Model Evaluation

Evaluation metrics included:

* **ROC-AUC** – performed in order to measure class separation ability.
* **Precision, Recall, F1-score** – performed in order to assess minority-class performance.
* **Confusion Matrix** – performed in order to understand error types.

Accuracy was intentionally de-emphasized due to class imbalance.

---

## 8. Threshold Optimization

Instead of relying on the default 0.5 probability threshold:

* Predicted probabilities were analyzed.
* Multiple thresholds were tested.
* A custom threshold was selected to **maximize recall for attrition**, while maintaining acceptable precision.

This ensured the model aligned with HR priorities (catching potential leavers early).

---

## 9. Model Interpretation with SHAP

To move beyond black-box predictions:

### 9.1 SHAP Value Analysis

* SHAP was applied primarily to the tree-based models.
* Global SHAP plots identified the most influential features driving attrition.

Key drivers included:

* Overtime
* Monthly income
* Job satisfaction
* Years at company

### 9.2 Business Insight Translation

SHAP results were translated into actionable insights, such as:

* Reducing excessive overtime
* Reviewing compensation bands
* Targeted retention strategies for early-tenure employees

---

## 10. Model Persistence

Final models were saved using `joblib`:

* Logistic Regression pipeline
* Random Forest pipeline
* XGBoost pipeline

Saving full pipelines ensured:

* Preprocessing and models remain tightly coupled
* Safe reuse during inference or future deployment

---

## 11. Key Outcomes

* Built a leakage-free, interpretable attrition prediction system
* Balanced performance with explainability
* Enabled probability-based decision-making via threshold tuning
* Delivered business-ready insights through SHAP

---

## 12. Tools & Technologies

* Python
* Pandas, NumPy
* Scikit-learn
* XGBoost
* SHAP
* Matplotlib / Seaborn
* Joblib

---

## 13. Conclusion

This project demonstrates a **production-minded data science workflow**, emphasizing:

* Correct evaluation practices
* Interpretability
* Business alignment

The result is a robust HR analytics solution that goes beyond prediction to deliver meaningful organizational insights.
