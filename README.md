# Telecom Customer Churn Prediction

A machine learning project for analyzing telecom customer churn and predicting customers who are likely to discontinue their services.

The project covers the complete workflow from raw-data exploration and cleaning to exploratory analysis, feature engineering, classification, and model evaluation.

## Project Objective

The objective is to:

- Understand customer characteristics associated with churn.
- Explore relationships between customer services, billing, contracts, tenure, and churn.
- Prepare the dataset for machine learning.
- Compare Logistic Regression, Decision Tree, and KNN classifiers.
- Generate churn predictions and evaluate model performance.

## Dataset

The project uses the IBM Telco Customer Churn dataset.

- Records: **7,043 customers**
- Original features: **21**
- Target: `Churn`
- Target classes:
  - `No`
  - `Yes`

The raw dataset is kept locally and is excluded from Git tracking.

## Project Workflow

```text
Raw Dataset
    ↓
01. Data Exploration
    ↓
02. Data Cleaning
    ↓
03. Exploratory Data Analysis
    ↓
04. Feature Engineering
    ↓
05. Model Analysis
    ↓
Churn Predictions
```

## Project Structure

```text
Telecome_churn_prediction/
│
├── data/
│   ├── raw/
│   └── processed/
│
├── docs/
│   ├── HLD.md
│   └── LLD.md
│
├── notebooks/
│   ├── 01_data_exploration.ipynb
│   ├── 02_cleaning.ipynb
│   ├── 03_eda.ipynb
│   ├── 04_feature_engineering.ipynb
│   └── 05_model_analysis.ipynb
│
├── reports/
│   ├── final_report/
│   │   └── telecom_customer_churn_final_report.md
│   └── Phase_report/
│       ├── data_exploration_report.md
│       └── eda_report.md
│
├── .gitignore
├── README.md
└── requirements.txt
```

## Phase 1: Data Exploration

The raw dataset was inspected for:

- Dataset shape and structure
- Data types
- Missing values
- Blank values
- Duplicate customer IDs
- Numerical ranges
- Categorical values
- Logical relationships between service variables
- Target distribution

### Important finding

`TotalCharges` was stored as an object even though it represents a numerical value.

There were **11 blank `TotalCharges` records**. These records had zero tenure and were preserved during cleaning.

No duplicate customer IDs or logical dependency violations were identified.

## Phase 2: Data Cleaning

Cleaning was performed based on the findings from the exploration stage.

Main operations:

- Converted `TotalCharges` to numeric.
- Handled blank `TotalCharges` values.
- Preserved valid customer records.
- Verified duplicate IDs.
- Verified categorical values and service dependencies.
- Validated the final dataset.

Final cleaned dataset:

- **7,043 rows**
- **21 columns**
- No missing values after cleaning.

## Phase 3: Exploratory Data Analysis

The EDA focused on variables with useful relationships to churn.

Important observations:

- Overall churn rate: **26.53%**
- Month-to-month contract churn rate: **42.71%**
- One-year contract churn rate: **11.27%**
- Two-year contract churn rate: **2.83%**
- Fiber optic churn rate: **41.89%**
- Electronic check churn rate: **45.29%**
- Customers without Tech Support: **41.64%** churn rate
- Customers without Online Security: **41.77%** churn rate
- Tenure and TotalCharges correlation: **0.83**

These are observed associations in the dataset and should not be interpreted as causal relationships.

## Phase 4: Feature Engineering

The cleaned dataset was prepared for classification.

### Processing

- Removed `customerID`.
- Encoded `Churn`:
  - `No → 0`
  - `Yes → 1`
- One-hot encoded categorical variables.
- Split the data using an 80/20 stratified train-test split.
- Standardized numerical features for Logistic Regression and KNN.

### Final dataset

- Training records: **5,634**
- Testing records: **1,409**
- Machine-learning features: **30**

The test set was kept separate during scaling to avoid data leakage.

## Phase 5: Model Analysis

Three classification algorithms were evaluated:

1. Logistic Regression
2. Decision Tree
3. K-Nearest Neighbors (KNN)

### Evaluation Metrics

The models were evaluated using:

- Accuracy
- Precision
- Recall
- F1-score
- ROC-AUC
- Confusion Matrix
- ROC Curve

### Results

| Model | Accuracy | Precision | Recall | F1 Score | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| Logistic Regression | **0.807** | **0.660** | 0.561 | **0.607** | **0.842** |
| Decision Tree | 0.726 | 0.484 | 0.479 | 0.481 | 0.647 |
| KNN | 0.767 | 0.560 | **0.564** | 0.562 | 0.792 |

Logistic Regression achieved the strongest overall performance across the evaluated metrics.

KNN produced slightly higher churn recall than Logistic Regression on this test set, while Logistic Regression achieved higher precision, F1-score, accuracy, and ROC-AUC.

## Prediction Output

The model analysis produces:

```text
data/processed/churn_predictions.csv
```

The prediction file contains:

- Test-set customer features
- `Actual_Churn`
- `Predicted_Churn`
- `Churn_Probability`

## Technologies

- Python
- Pandas
- NumPy
- Scikit-learn
- Matplotlib
- Seaborn
- Jupyter Notebook

## Reports

The `reports/` directory contains:

- Phase-wise exploration and EDA reports
- Final project report containing the complete analysis, modelling results, findings, and limitations.

## Limitations

- Evaluation uses a single stratified train-test split.
- Baseline model configurations were used without extensive hyperparameter tuning.
- EDA relationships represent association rather than causation.
- The dataset represents a particular telecom customer population.
- No external validation dataset was used.
- The prediction threshold was not optimized for a specific business cost function.

## Conclusion

The project demonstrates an end-to-end telecom customer churn classification workflow.

The analysis found substantial differences in churn rates across contract types, tenure levels, internet services, payment methods, technical support, and online security.

Among the three evaluated models, Logistic Regression achieved the strongest overall performance on the held-out test set, with **80.70% accuracy** and **0.842 ROC-AUC**.

The project is intended as a practical demonstration of customer churn analysis, feature preparation, classification, and model evaluation.
