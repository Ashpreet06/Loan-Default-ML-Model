# Loan Default / Credit Risk Prediction — Machine Learning Project

## 📌 Project Overview

This project uses machine learning to predict whether a loan applicant is likely to **default**.

The goal is to help lenders assess credit risk more consistently by using applicant information such as income, loan amount, interest rate, credit score, debt-to-income ratio, and age.

The notebook uses **one real, publicly available loan dataset from Kaggle** (`Loan_Default_csv.xlsx`) and builds a complete end-to-end credit-risk prediction workflow.

---

## 🎯 Business Problem

**Problem:** Predict whether a loan applicant is likely to default.

**Business use:** A lender can use the model as a decision-support tool to identify applicants who may require closer credit-risk assessment or manual underwriting.

> The model is a decision-support system, not a replacement for responsible human lending decisions.

---

## 📊 Dataset

**Dataset:** `Loan_Default_csv.xlsx`

The notebook describes the source dataset as containing approximately **148,670 mortgage/loan applicants and 34 columns**.

Relevant fields used in the project include:

- Age
- Income
- Loan amount
- Interest rate
- Credit score
- Debt-to-income ratio
- Loan default status

The target variable is:

- `1` = Default
- `0` = No default

---

## 🔄 Project Workflow

The notebook follows this pipeline:

**Filtering → Dataset Creation → Realistic Check → Cleaning & Preprocessing → Feature Engineering → Feature Selection → 70/30 Train-Test Split → Train 3 Models → Evaluation → Feature Importance → New Applicant Prediction → Interactive Demo → Business Impact Analysis → Risk Tiering → Executive Summary**

---

## 🧹 Data Preparation

The project performs several preprocessing steps:

1. **Column filtering**
   - Keeps relevant credit-risk variables.

2. **Row filtering**
   - Removes unusable records such as rows with non-positive income.

3. **Schema creation**
   - Renames/maps raw variables into a consistent modeling structure.

4. **Age transformation**
   - Converts age bands such as `25-34` into numeric midpoint values.

5. **Income transformation**
   - Converts monthly income into annual income.

6. **Missing-value treatment**
   - Uses median imputation for selected numeric variables.

7. **Outlier treatment**
   - Clips selected financial variables at the 1st and 99th percentiles.

---

## 🧠 Feature Engineering

The notebook creates additional features to capture useful credit-risk relationships:

- **Loan-to-income ratio**
  - `loan_amount / annual_income`
  - Represents loan size relative to annual income.

- **Normalized credit score**
  - Rescales the credit score to a 0–100 range.

- **High DTI flag**
  - `1` when debt-to-income ratio is above 40%; otherwise `0`.

- **Age group**
  - Groups applicants into age ranges for analysis.

---

## 🔎 Feature Selection

Two approaches are used:

### 1. Correlation Analysis
The project checks how candidate variables correlate with the default target.

### 2. Random Forest Feature Importance
A Random Forest is used to estimate the relative importance of candidate features.

The notebook then selects the **top 7 features** for model training.

---

## 🤖 Machine Learning Models

Three binary-classification models are trained and compared:

### 1. Logistic Regression
Used as an interpretable statistical baseline.

### 2. Decision Tree
Uses rule-based splits and is relatively easy to interpret and visualize.

### 3. Random Forest
Combines multiple decision trees to capture more complex patterns.

For a fair comparison, all three models use the **same 70/30 train-test split**.

Logistic Regression uses feature standardization, while the tree-based models do not require scaling.

---

## 📏 Model Evaluation

The notebook evaluates the models using:

- Accuracy
- Precision
- Recall
- F1-score
- ROC-AUC
- Confusion matrix
- Classification report
- ROC curve

### Cross-Validation

A **5-fold stratified cross-validation** check is also included.

It reports the mean ROC-AUC and standard deviation across five folds, giving a more reliable view of model performance than relying only on one train-test split.

---

## ⚖️ Handling Class Imbalance

Loan default is a minority outcome in the dataset.

Because accuracy alone can be misleading when one class is much more common, the project also trains versions of the models using:

`class_weight="balanced"`

This gives greater importance to the minority default class during training and allows comparison between the original and balanced approaches.

---

## 💰 Business Impact / Cost-Benefit Analysis

The project goes beyond technical metrics and considers the financial impact of prediction errors.

### False Negative
The model predicts an applicant as safe, but the applicant actually defaults.

The notebook treats this as a potentially high-cost error because the lender may lose the loan principal.

### False Positive
The model predicts an applicant as risky, but the applicant would have repaid.

The notebook estimates the associated cost using expected interest income.

This demonstrates an important business principle:

**A model should not be evaluated only by accuracy; the financial consequences of different errors also matter.**

---

## 👤 New Applicant Prediction

The notebook includes a helper function that accepts human-readable applicant information:

- Age
- Annual income
- Loan amount
- Interest rate
- Credit score
- Debt-to-income ratio

The function automatically calculates the engineered features required by the trained models.

A sample applicant is then evaluated by all three models, producing:

- Prediction
- Estimated probability of default

---

## 🎛️ Interactive Demo

An interactive `ipywidgets` demo is included.

Users can change:

- Age
- Annual income
- Loan amount
- Interest rate
- Credit score
- Debt-to-income ratio

The notebook automatically recalculates the engineered features and displays predictions from all three models.

---

## 📊 Risk Tiering

The notebook converts predicted default probabilities into three simple risk tiers:

- **Low Risk:** probability of default < 20%
- **Medium Risk:** 20%–50%
- **High Risk:** > 50%

The purpose is to demonstrate how a lender could move from a simple prediction toward a risk-management workflow.

Possible business actions described in the notebook include standard treatment, additional pricing for higher risk, or manual underwriting review.

---

## 🛠️ Technologies Used

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- OpenPyXL
- IPyWidgets
- Google Colab / Jupyter Notebook

---

## ▶️ How to Run

### Step 1 — Open the notebook
Open the `.ipynb` file in **Google Colab** or Jupyter Notebook.

### Step 2 — Install/import libraries
Run the first cell to install/import the required Python packages.

### Step 3 — Add the dataset
Upload:

`Loan_Default_csv.xlsx`

to the same Colab session or make it accessible through Google Drive.

### Step 4 — Run the notebook
Run the cells from top to bottom so that preprocessing, feature engineering, model training, evaluation, prediction, and business analysis are executed in order.

---

## 📁 Project Structure

```text
Loan Default ML Project/
│
├── loan default ml models (1).ipynb
├── Loan_Default_csv.xlsx
└── README.md
```

---

## ⚠️ Limitations

- The model is trained on the selected dataset and should not automatically be assumed to perform equally well on another lender's portfolio.
- Age bands are represented using midpoint values, which is an approximation.
- The business cost assumptions in the notebook are simplified estimates.
- Risk thresholds such as 20%, 50%, and the 40% DTI flag are used as a simple demonstration structure.
- Model predictions should be treated as **decision-support outputs**, not automatic approval or rejection decisions.
- Real-world deployment would require additional validation, monitoring, fairness checks, data-quality controls, and governance.

---

## 📌 Key Takeaway

This project demonstrates a complete machine-learning approach to **loan default prediction**, from raw financial data and preprocessing to model comparison, class-imbalance handling, explainability, applicant-level prediction, and business impact analysis.

The main idea is simple:

**Use machine learning not just to predict default risk, but to turn those predictions into useful and explainable credit-risk insights.**
