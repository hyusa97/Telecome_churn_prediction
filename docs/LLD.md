# Low-Level Design (LLD)
## Telecom Customer Churn Prediction

## 1. System Design

The project follows a simple sequential workflow based on the observed dataset:

```text
Raw Dataset
   ↓
01. Data Exploration
   ↓
Data Cleaning
   ↓
Processed CSV
   ↓
02. Exploratory Data Analysis
   ↓
03. Feature Engineering
   ↓
04. Model Evaluation
   ↓
Churn Predictions & Analysis
```

Cleaning decisions are made after the raw data has been explored. The cleaning stage therefore responds to the actual dataset rather than assuming cleaning rules in advance.

---

## 2. Data Exploration

### Responsibility

Understand the structure and quality of the raw IBM Telco Customer Churn dataset before making cleaning decisions.

### Main Activities

- Load the raw CSV using Python/Pandas.
- Inspect dataset shape and columns.
- Inspect data types.
- Check missing values and blank values.
- Check duplicate customer records.
- Inspect numerical variables and ranges.
- Inspect categorical values.
- Check the target variable and class distribution.
- Identify data-quality issues and logical inconsistencies.

The findings from this notebook determine what needs to be cleaned.

The original source dataset in `data/raw/` remains unchanged.

---

## 3. Data Cleaning

### Responsibility

Apply only the cleaning operations identified during data exploration and produce a clean dataset for the rest of the project.

### Cleaning Flow

```text
Raw Dataset
   ↓
Cleaning based on exploration findings
   ↓
Data Type Correction
   ↓
Missing / Blank Value Handling
   ↓
Required Validation
   ↓
Clean Dataset
   ↓
data/processed/
```

### Main Activities

Depending on the findings from the exploration stage:

- Correct inappropriate data types.
- Handle blank or missing values.
- Remove or handle duplicates if required.
- Correct or standardize inconsistent categorical values if present.
- Apply necessary validation checks.
- Preserve the target variable and required customer information.

Cleaning decisions should be kept simple, explicit, and reproducible.

### Output

The cleaned dataset will be saved as a CSV file in:

```text
data/processed/
```

This processed CSV becomes the input for EDA and the later project stages.

---

## 4. Exploratory Data Analysis

### Responsibility

Analyze the cleaned dataset to understand relationships between customer characteristics and churn.

### Main Areas

- Overall churn distribution.
- Contract type vs churn.
- Tenure vs churn.
- Monthly charges vs churn.
- Internet service vs churn.
- Payment method vs churn.
- Relevant service features vs churn.
- Important relationships or interactions identified during analysis.

EDA will focus on useful questions rather than producing plots for every column. It will remain proportionate to the size and complexity of the dataset.

---

## 5. Feature Engineering

### Responsibility

Transform the cleaned dataset into features suitable for machine-learning models.

```text
Cleaned Dataset
      ↓
Feature Engineering
      ↓
Feature Matrix (X)
+
Target Variable (y)
```

### Operations

Depending on the analysis and model requirements:

- Select relevant features.
- Encode categorical variables.
- Create useful derived features where justified.
- Apply numerical transformations where required.
- Scale features where required, particularly for distance-based models.
- Remove unsuitable or leakage-prone variables.

Feature engineering must not use information that would only become available after the churn outcome.

---

## 6. Machine Learning

Three classification algorithms will be implemented:

```text
                  Feature Dataset
                       |
           +-----------+-----------+
           |           |           |
           v           v           v
      Logistic     Decision       KNN
     Regression      Tree
           |           |           |
           +-----------+-----------+
                       |
                   Evaluation
```

### 6.1 Logistic Regression

Used as a baseline binary classification model.

### 6.2 Decision Tree

Used to capture nonlinear relationships and interactions between features.

### 6.3 K-Nearest Neighbors

Used as a distance-based classification model. Feature scaling will be applied where required.

---

## 7. Model Preparation and Evaluation

### Training Flow

```text
Feature Dataset
      ↓
Feature / Target Separation
      ↓
Train-Test Split
      ↓
Preprocessing
      ↓
Model Training
      ↓
Predictions
      ↓
Evaluation
```

Where appropriate, preprocessing such as encoding and scaling should be handled reproducibly so that the same transformations are applied correctly to training and test data.

### Evaluation Metrics

- Accuracy
- Precision
- Recall
- F1-score
- ROC-AUC
- Confusion Matrix

Because the churn classes are imbalanced, accuracy alone will not be treated as sufficient.

---

## 8. Model Comparison

The three models will be evaluated using the same:

- Dataset split.
- Target definition.
- Feature set.
- Evaluation metrics.

Results will be summarized in a comparison table containing:

```text
Model
  ├── Accuracy
  ├── Precision
  ├── Recall
  ├── F1-score
  └── ROC-AUC
```

The comparison will describe the observed performance of each model without relying on a single metric.

---

## 9. Prediction Output

```text
Customer Features
       ↓
Trained Model
       ↓
Churn Probability
       ↓
Predicted Churn Class
```

The output may contain:

- Customer ID
- Churn probability
- Predicted churn class

---

## 10. Project Directory Mapping

```text
telecom-customer-churn/
│
├── data/
│   ├── raw/
│   ├── interim/
│   └── processed/
│
├── docs/
│   ├── HLD.md
│   └── LLD.md
│
├── notebooks/
│   ├── 01_data_exploration.ipynb
│   ├── 02_eda.ipynb
│   ├── 03_feature_engineering.ipynb
│   └── 04_model_evaluation.ipynb
│
├── reports/
│   ├── figures/
│   └── final_report/
│
├── README.md
├── requirements.txt
└── .gitignore
```

---

## 11. Technology Mapping

| Component | Technology |
|---|---|
| Data exploration | Python, Pandas |
| Data cleaning | Python, Pandas |
| EDA | Python, Pandas, Matplotlib, Seaborn |
| Feature engineering | Python, Pandas, Scikit-learn |
| Classification | Scikit-learn |
| Evaluation | Scikit-learn, Python |
| Version control | Git |
| Repository | GitHub |

---

## 12. Implementation Flow

```text
1. Raw Dataset
      ↓
2. Data Exploration
      ↓
3. Dataset-specific Cleaning
      ↓
4. Clean Processed CSV
      ↓
5. Exploratory Data Analysis
      ↓
6. Feature Engineering
      ↓
7. Train/Test Preparation
      ↓
8. Logistic Regression
      ↓
9. Decision Tree
      ↓
10. KNN
      ↓
11. Model Evaluation
      ↓
12. Model Comparison
      ↓
13. Churn Prediction
      ↓
14. Reporting
```

---

## 13. Design Boundary

This LLD defines the project's workflow and responsibilities without prescribing cleaning rules before the dataset has been examined.

The following details are intentionally determined during implementation:

- Dataset-specific cleaning decisions.
- Exact feature definitions.
- Encoding choices.
- Scaling choices.
- Model hyperparameters.
- Final model results.

The project uses the notebooks and processed dataset directly rather than introducing additional source-code, SQL, or warehouse layers that are not required for this implementation.
