# Data Exploration Report

## 1. Overview

This report documents the initial structural and data-quality exploration of the IBM Telco Customer Churn dataset before dedicated exploratory data analysis (EDA).

- **Rows:** 7,043
- **Columns:** 21
- **Target:** `Churn`
- **Grain:** Customer-level records

## 2. Dataset Structure

The dataset contains 18 `object` columns, 2 `int64` columns, and 1 `float64` column.

Important observations:
- `customerID` is the customer identifier.
- `Churn` is the binary classification target.
- `SeniorCitizen` is encoded as 0/1.
- `tenure` represents customer tenure in months.
- `MonthlyCharges` is numeric.
- `TotalCharges` contains numeric-looking values but is stored as `object`.

## 3. Missing-Value Investigation

A standard null-value check found **0 Pandas `NaN` values**.

A separate whitespace check found **11 blank values**, all in `TotalCharges`.

The 11 affected records were inspected using:
- `customerID`
- `tenure`
- `MonthlyCharges`
- `TotalCharges`
- `Churn`

All 11 records had:
- `tenure = 0`
- `TotalCharges = blank`
- `MonthlyCharges > 0`
- `Churn = No`

### Decision

These customers will **not be dropped automatically**. The records appear to represent customers with zero tenure for whom accumulated charges have not yet been recorded.

During cleaning, the blank `TotalCharges` values will be handled explicitly and the column converted to a numeric type.

## 4. Duplicate Investigation

Duplicate customer IDs were checked.

- **Duplicate customer IDs:** 0
- **Total records:** 7,043

Therefore, `customerID` is unique in the dataset.

## 5. Target Variable

### `Churn`

| Churn | Customers | Percentage |
|---|---:|---:|
| No | 5,174 | 73.47% |
| Yes | 1,869 | 26.53% |
| **Total** | **7,043** | **100%** |

The target has moderate class imbalance. Therefore, accuracy should not be used as the only model evaluation metric.

Evaluation should include:
- Precision
- Recall
- F1-score
- ROC-AUC
- Confusion matrix

## 6. Numerical Variable Investigation

### `SeniorCitizen`

| Statistic | Value |
|---|---:|
| Count | 7,043 |
| Mean | 0.1621 |
| Std | 0.3686 |
| Min | 0 |
| 25% | 0 |
| Median | 0 |
| 75% | 0 |
| Max | 1 |

`SeniorCitizen` is numerically encoded but semantically represents a binary attribute.

### `tenure`

| Statistic | Value |
|---|---:|
| Count | 7,043 |
| Mean | 32.37 |
| Std | 24.56 |
| Min | 0 |
| 25% | 9 |
| Median | 29 |
| 75% | 55 |
| Max | 72 |

### `MonthlyCharges`

| Statistic | Value |
|---|---:|
| Count | 7,043 |
| Mean | 64.76 |
| Std | 30.09 |
| Min | 18.25 |
| 25% | 35.50 |
| Median | 70.35 |
| 75% | 89.85 |
| Max | 118.75 |

No immediately invalid range was identified from these descriptive statistics.

## 7. Categorical Variable Investigation

The inspected categorical variables contained consistent, meaningful categories.

### Customer attributes

| Column | Categories |
|---|---|
| `gender` | Male, Female |
| `Partner` | Yes, No |
| `Dependents` | Yes, No |

### Service attributes

| Column | Categories |
|---|---|
| `PhoneService` | Yes, No |
| `MultipleLines` | Yes, No, No phone service |
| `InternetService` | DSL, Fiber optic, No |
| `OnlineSecurity` | Yes, No, No internet service |
| `OnlineBackup` | Yes, No, No internet service |
| `DeviceProtection` | Yes, No, No internet service |
| `TechSupport` | Yes, No, No internet service |
| `StreamingTV` | Yes, No, No internet service |
| `StreamingMovies` | Yes, No, No internet service |
| `PaperlessBilling` | Yes, No |

### Contract and payment

| Column | Categories |
|---|---|
| `Contract` | Month-to-month, One year, Two year |
| `PaymentMethod` | Electronic check, Mailed check, Bank transfer (automatic), Credit card (automatic) |

No unexpected category values were observed during the initial inspection.

## 8. Data Integrity Checks

### Phone service dependency

Expected relationship:

> If `PhoneService = No`, then `MultipleLines = No phone service`.

**Result: 0 violations.**

### Internet service dependencies

Expected relationship:

> If `InternetService = No`, internet-dependent service variables should contain `No internet service`.

Checked:
- `OnlineSecurity`
- `OnlineBackup`
- `DeviceProtection`
- `TechSupport`
- `StreamingTV`
- `StreamingMovies`

**Result: 0 violations across all checked variables.**

These logical dependencies are consistent in the dataset.

## 9. Key Findings

1. The dataset contains **7,043 customers and 21 variables**.
2. Customer IDs are unique.
3. No Pandas `NaN` values were detected.
4. **11 whitespace-only values exist in `TotalCharges`**.
5. All 11 affected records have `tenure = 0`.
6. `TotalCharges` requires numeric conversion during cleaning.
7. `Churn` contains two valid classes.
8. Churn represents approximately **26.5%** of customers.
9. The target therefore has moderate class imbalance.
10. No unexpected categories were identified in the inspected categorical variables.
11. Logical dependencies between phone/internet service variables were verified with **zero violations**.
12. No immediately invalid ranges were identified in the initially numeric variables.

## 10. Points to Carry Forward

### Data Cleaning
- Handle the 11 blank `TotalCharges` values explicitly.
- Convert `TotalCharges` to numeric.
- Preserve the affected customer records unless later evidence indicates otherwise.
- Review semantic data types before modeling.

### EDA
Investigate relationships between churn and:
- Tenure
- Contract type
- Monthly charges
- Total charges
- Internet/service configuration
- Payment method
- Customer demographics
- Interactions between important variables

### Machine Learning
Because the target is moderately imbalanced:
- Do not rely on accuracy alone.
- Include precision, recall, F1-score, ROC-AUC, and confusion matrices.
- Prevent data leakage during preprocessing and feature engineering.

