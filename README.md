# AI for Credit Risk Assessment in NBFC Lending

## Loan Default Prediction Using Machine Learning

A Business AI/ML case study on using machine learning to assess credit risk and predict the likelihood of loan default in India's NBFC lending sector.

---

## 📌 Project Overview

This project demonstrates a complete **loan default prediction pipeline** using two real, publicly available lending datasets.

The project combines:

- Data cleaning and filtering
- Dataset harmonization
- Feature engineering
- Feature selection
- Supervised machine learning
- Model comparison
- Cross-validation
- Class-imbalance handling
- Feature-importance analysis
- New-applicant prediction
- Interactive prediction
- Cost-benefit analysis
- Risk tiering
- Responsible AI and regulatory considerations

The objective is to show how AI/ML can support lenders in making **faster, more consistent and data-driven credit-risk decisions**.

> **Important:** This is an academic prototype and demonstration, not a production-ready lending system.

---

## 🎯 Business Problem

NBFCs provide loans to retail borrowers, small businesses, self-employed individuals and other customers, including people with limited or thin credit histories.

Traditional lending processes can face challenges such as:

- Limited credit history for some borrowers
- Slow manual underwriting
- Large volumes of small-ticket digital loans
- Inconsistent human judgement
- Difficulty identifying potential defaulters early

### Problem Statement

**Can AI/ML predict whether a loan applicant is likely to default and help an NBFC assess credit risk more effectively?**

---

## 🎯 Project Objectives

1. Understand how AI/ML can be applied to credit-risk assessment.
2. Combine and prepare real lending datasets for analysis.
3. Engineer meaningful financial-risk features.
4. Train multiple classification models for loan-default prediction.
5. Compare model performance using multiple evaluation metrics.
6. Examine class imbalance and its business implications.
7. Predict default risk for new applicants.
8. Translate model errors into estimated business costs.
9. Create risk tiers based on predicted probability of default.
10. Consider explainability, fairness, privacy, human oversight and regulatory requirements.

---

## 📂 Project Files

| File | Description |
|---|---|
| `loan default ml models.ipynb` | Main Jupyter/Google Colab notebook containing the complete ML workflow |
| `credit_risk_dataset.csv` | First public lending dataset used in the project |
| `Loan_Default.csv.xlsx` | Second public lending dataset used in the project |
| `README.md` | Project documentation |

### ⚠️ Notebook filename note

The notebook's loading cell currently refers to the second dataset as:

`Loan_Default_csv.xlsx`

If your GitHub file is named `Loan_Default.csv.xlsx`, either rename the file to match the notebook or update the filename in the notebook's data-loading cell.

---

## 📊 Data Preparation

The project combines two datasets with different schemas, units and encodings.

Before modelling, the notebook performs:

### 1. Row Filtering

Invalid or unusable records are removed, including:

- Unrealistic ages
- Non-positive income
- Invalid employment length
- Non-positive loan amounts

### 2. Column Filtering

Only fields that can be meaningfully mapped to the common credit-risk schema are retained from the second dataset.

### 3. Dataset Harmonization

The datasets are mapped into a shared structure.

Examples include:

- Converting age ranges into numeric midpoints
- Converting letter loan grades into approximate numeric score values
- Annualizing monthly income
- Converting loan-percent-income into debt-to-income percentage

These transformations are documented in the notebook.

### 4. Missing Values

Missing numerical values are handled using **median imputation**.

A separate `employment_length_known` flag is also created to preserve information about whether employment length was reported.

### 5. Outlier Handling

Financial variables are checked and extreme values are clipped using percentile-based limits.

---

## 🧮 Feature Engineering

The notebook creates additional variables that represent financial risk and affordability.

### Loan-to-Income Ratio

`loan_to_income_ratio = loan_amount / annual_income`

This represents the requested loan relative to the applicant's annual income.

### Normalized Credit Score

Credit scores from the two sources are converted to a common 0–100 scale.

### High-DTI Flag

`high_dti_flag = 1` when debt-to-income is above 40%; otherwise `0`.

### Employment-Length Indicator

`employment_length_known` records whether employment length was available.

---

## 🤖 Machine Learning Models

Loan default is treated as a **binary classification problem** using supervised learning.

Three models are trained:

### 1. Logistic Regression

Used as an interpretable baseline model.

Its coefficients help show the direction and relative strength of relationships between features and predicted risk.

### 2. Decision Tree

A tree-based model capable of capturing non-linear relationships through a sequence of decision rules.

### 3. Random Forest

An ensemble of multiple decision trees that can capture more complex patterns.

The notebook uses:

- 300 trees
- Maximum depth of 10
- Minimum leaf-size constraints

---

## 🔬 Train/Test Method

The same **70/30 stratified train-test split** is used for the three models so their results can be compared fairly.

A `random_state` of 42 is used for reproducibility.

Logistic Regression uses feature standardization, while the tree-based models do not require scaling.

---

## 📏 Model Evaluation

The models are evaluated using:

### Accuracy
Overall percentage of correct predictions.

### Precision
Among applicants predicted as defaulters, the proportion who actually defaulted.

### Recall
Among actual defaulters, the proportion correctly identified by the model.

### F1 Score
A combined measure balancing precision and recall.

### ROC-AUC
Measures how well the model separates defaulters from non-defaulters across different classification thresholds.

The notebook also generates:

- Classification reports
- Confusion matrices
- ROC curves

---

## 🔁 Cross-Validation

The project uses **5-fold stratified cross-validation** using ROC-AUC.

Instead of relying on only one train/test split, each model is retrained across multiple folds and the mean and standard deviation of ROC-AUC are calculated.

This provides a more reliable view of model performance.

---

## ⚖️ Class Imbalance

The combined dataset contains substantially more non-default cases than default cases.

This can make accuracy misleading because a model could achieve high accuracy by predicting "no default" too often.

To address this, the notebook retrains the models using:

`class_weight="balanced"`

This gives greater importance to the minority default class and allows comparison of the resulting precision/recall trade-off.

---

## 🔍 Feature Importance

The notebook examines feature importance using different approaches:

- **Logistic Regression:** standardized coefficients
- **Decision Tree:** impurity-based feature importance
- **Random Forest:** averaged feature importance across trees

The `source` field is deliberately excluded from modelling so that the model cannot simply learn which original dataset a record came from.

---

## 👤 New Applicant Prediction

The notebook includes a helper function that accepts raw, human-readable applicant information such as:

- Age
- Annual income
- Loan amount
- Interest rate
- Credit score
- Debt-to-income percentage
- Employment length

The function automatically creates the engineered features required by the trained models.

The three models can then produce:

- Predicted outcome
- Estimated probability of default

This demonstrates how the modelling pipeline could support a hypothetical lending workflow.

---

## 🖥️ Interactive Demo

The notebook also includes an interactive demonstration using widgets.

Users can change applicant inputs such as:

- Age
- Income
- Loan amount
- Interest rate
- Credit score
- DTI
- Employment length

The predictions from the models update based on the entered applicant profile.

---

## 💰 Cost-Benefit Analysis

The project goes beyond model accuracy by examining the **business cost of prediction errors**.

### False Negative

The model predicts an applicant as safe, but the applicant actually defaults.

The notebook treats this as a high-cost error because the lender may lose the loan principal.

### False Positive

The model predicts an applicant as risky, but the applicant would have repaid.

The notebook estimates this cost using the potential interest income that could have been earned.

This reframes model evaluation from simply asking:

> "Which model is more accurate?"

to also asking:

> **"What could prediction errors cost the lending business?"**

---

## 📈 Risk Tiering

Instead of using only an automatic approve/reject decision, the notebook groups applicants according to predicted probability of default:

| Risk Tier | Probability of Default |
|---|---:|
| Low Risk | < 20% |
| Medium Risk | 20%–50% |
| High Risk | > 50% |

These tiers are presented as a simple demonstration of how predictions could support different lending treatments.

For example:

- Low risk → standard lending treatment
- Medium risk → additional risk assessment or risk-based pricing
- High risk → manual underwriting/review

These thresholds are illustrative and should not be treated as universal lending rules.

---

## 🛡️ Responsible AI Considerations

Credit decisions can directly affect people, so the project considers several responsible-AI issues.

### Data Privacy & Consent
Financial information must be collected and used under appropriate privacy and consent requirements.

### Bias & Fairness
The model should be tested for unintended disparities across demographic groups before any real deployment.

The notebook intentionally avoids protected characteristics in its modelling features, but it also notes that a full disparate-impact/fairness analysis was **not performed**.

### Explainability
Credit decisions may require understandable explanations, making model interpretability an important consideration.

### Human Oversight
Borderline and high-value applications should receive human review rather than relying entirely on automated predictions.

### Ongoing Monitoring
A deployed model would need periodic validation and retraining because borrower behaviour and market conditions can change.

---

## ⚠️ Limitations

This project is an academic prototype and has important limitations:

1. The two source datasets use different definitions and scales for some variables.
2. Some variables were harmonized using approximations.
3. Letter-based grades were mapped to approximate numeric credit scores.
4. Monthly income was annualized where required.
5. The project does not represent a production NBFC underwriting system.
6. Full fairness/disparate-impact testing was not performed.
7. Regulatory approval and production model governance are outside the scope of this notebook.
8. Real lending deployment would require a consistent, governed data pipeline and additional validation.

---

## 📋 Key Findings / Learning

The project demonstrates three major learning points:

### 1. Model Choice Is a Trade-Off

Different models offer different balances between predictive performance and interpretability.

### 2. Better Prompts Improve AI-Assisted Research

Specific prompts containing a role, industry scope, requested information and source requirements produced more useful research output.

### 3. AI Output Must Be Verified

An early AI response contained generic global adoption figures without a source. These were checked against the RBI Financial Stability Report and industry publications and replaced with sourced, India-specific information.

**AI was used as a research assistant, not as an unquestioned source of truth.**

---

## 🧠 Generative AI Used in the Research

| Tool | Used For |
|---|---|
| ChatGPT | Understanding AI/ML concepts and structuring the case study |
| Claude | Building and explaining the Python Logistic Regression model and analysing long research material |
| Perplexity | Finding and cross-checking sources and industry statistics |

### Example Prompt

> "Explain how Indian NBFCs use AI/ML for loan default prediction. Focus on the data sources used, the type of model, and evidence of real-world adoption. Provide reliable sources."

The project also demonstrates **prompt refinement**, changing a broad question into a specific, India-focused research request.

---

## 🔄 Complete Project Pipeline

```text
Real Lending Datasets
        ↓
Filtering
        ↓
Dataset Harmonization
        ↓
Data Cleaning & Preprocessing
        ↓
Exploratory Analysis
        ↓
Feature Engineering
        ↓
Feature Selection
        ↓
70/30 Train-Test Split
        ↓
Logistic Regression
Decision Tree
Random Forest
        ↓
Model Evaluation
        ↓
Cross-Validation
        ↓
Class-Imbalance Analysis
        ↓
Feature Importance
        ↓
New Applicant Prediction
        ↓
Cost-Benefit Analysis
        ↓
Risk Tiering
        ↓
Executive & Regulatory Interpretation
```

---

## 📚 References

The project documentation and presentation reference:

- RBI Financial Stability Report (2025)
- Indiafintech, *The Indian Bank's Guide to AI* (2026)
- AppWrk Insights, *AI in NBFC Lending* (2026)
- Biz2X, *AI Credit Decisioning for Banks & NBFCs* (2025)
- Malhotra et al., ResearchGate (2025)
- Kaggle: Credit Risk Dataset
- Kaggle: Loan Default Dataset

---

## 👥 Project Team

| Member |
|---|
| Anshika Kapoor |
| Aditya Thakur |
| Ashpreet Kaur |
| Khushmeet Kaur |
| Jashanpreet Kaur |
| Gaurav Chawla |

### GitHub Handles

- Anshika Kapoor — `anshikakapoor707-coder`
- Aditya Thakur — `Aditya2307-CODER`
- Ashpreet Kaur — `Ashpreet06`
- Khushmeet Kaur — `khushmeet07`
- Jashanpreet Kaur — `jashanpreetk2308-create`
- Gaurav Chawla — `gaurav2594bbafai25-ui`

---

## 🎓 Course

**AI & ML**

## 📌 Project Type

**Business AI/ML Case Study — Credit Risk Assessment and Loan Default Prediction**

---

## 💡 Final Takeaway

> **AI can make credit-risk assessment faster and more data-driven, but responsible lending requires appropriate data, suitable models, verification, explainability, continuous monitoring, and human oversight.**
