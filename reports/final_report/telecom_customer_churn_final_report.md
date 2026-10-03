# Telecom Customer Churn Analysis
## Exploratory Analysis, Feature Engineering and Classification

---

## 1. Project Overview

This project analyzes customer churn in a telecom dataset and develops classification models to identify customers who are likely to discontinue their services.

The project follows a simple sequential workflow:

```text
Raw Dataset
    ↓
Data Exploration
    ↓
Data Cleaning
    ↓
Exploratory Data Analysis
    ↓
Feature Engineering
    ↓
Model Evaluation
    ↓
Churn Predictions and Analysis
```

The dataset contains **7,043 customer records and 21 original variables**.

Three classification algorithms were evaluated:

- Logistic Regression
- Decision Tree
- K-Nearest Neighbors (KNN)

The objective was to compare their ability to distinguish customers who churned from those who remained.

---

# 2. Data Exploration and Cleaning

## 2.1 Dataset Structure

The original dataset contained:

- **7,043 rows**
- **21 columns**
- Numerical and categorical customer attributes
- `Churn` as the target variable

The original dataset contained no values detected by the standard `isnull()` check.

However, exploration identified **11 blank values in `TotalCharges`**.

These records had:

- `tenure = 0`
- blank `TotalCharges`
- positive `MonthlyCharges`
- `Churn = No`

`TotalCharges` was initially stored as an object/string column despite representing a numerical quantity.

## 2.2 Cleaning Decisions

The cleaning stage:

1. Converted `TotalCharges` to a numeric data type.
2. Handled the 11 blank `TotalCharges` values while preserving the corresponding customer records.
3. Verified duplicate customer IDs.
4. Verified categorical values.
5. Verified logical relationships between dependent service variables.
6. Verified numerical ranges.

No duplicate customer IDs or logical dependency violations were found during exploration.

The cleaned dataset retained **7,043 records and 21 columns**.

---

# 3. Exploratory Data Analysis

The EDA focused on identifying patterns associated with customer churn.

## 3.1 Churn Distribution

The target distribution was:

| Churn | Customers | Percentage |
|---|---:|---:|
| No | 5,174 | 73.47% |
| Yes | 1,869 | 26.53% |

The dataset therefore contains a moderate class imbalance.

This is important for model evaluation because accuracy alone may not adequately describe performance.

---

## 3.2 Contract Type and Churn

Observed churn rates:

| Contract Type | Churn Rate |
|---|---:|
| Month-to-month | 42.71% |
| One year | 11.27% |
| Two year | 2.83% |

Month-to-month customers had a substantially higher observed churn rate than customers with one-year or two-year contracts.

This indicates that contract type is an important variable to consider during predictive modelling.

---

## 3.3 Tenure and Churn

The tenure distributions show a clear difference between churned and retained customers.

Churned customers are more concentrated toward lower tenure values, while retained customers are more strongly represented at higher tenure values.

The boxplot also shows a substantially higher median tenure for customers who did not churn.

This indicates a strong association between customer tenure and churn.

---

## 3.4 Monthly Charges and Churn

The boxplot showed that churned customers generally have higher monthly charges than retained customers.

Approximate median values observed in the analysis:

- Non-churned customers: around 64
- Churned customers: around 80

There is substantial overlap between the two groups, so monthly charges alone do not clearly separate churned and retained customers.

---

## 3.5 Internet Service and Churn

Observed churn rates:

| Internet Service | Churn Rate |
|---|---:|
| Fiber optic | 41.89% |
| DSL | 18.96% |
| No internet service | 7.40% |

Fiber optic customers had the highest observed churn rate among the three groups.

This is an association in the observed data and should not be interpreted as evidence that the service type itself causes churn.

---

## 3.6 Payment Method and Churn

Observed churn rates:

| Payment Method | Churn Rate |
|---|---:|
| Electronic check | 45.29% |
| Mailed check | 19.11% |
| Bank transfer (automatic) | 16.71% |
| Credit card (automatic) | 15.24% |

Electronic check users had the highest observed churn rate among the payment methods examined.

---

## 3.7 Tech Support and Churn

Observed churn rates:

| Tech Support | Churn Rate |
|---|---:|
| No | 41.64% |
| Yes | 15.17% |
| No internet service | 7.40% |

Customers without Tech Support showed a substantially higher observed churn rate.

---

## 3.8 Online Security and Churn

Observed churn rates:

| Online Security | Churn Rate |
|---|---:|
| No | 41.77% |
| Yes | 14.61% |
| No internet service | 7.40% |

Customers without Online Security showed a substantially higher observed churn rate.

---

## 3.9 Numerical Correlations

The numerical correlation analysis produced:

| Variable Pair | Correlation |
|---|---:|
| Tenure - TotalCharges | 0.83 |
| MonthlyCharges - TotalCharges | 0.65 |
| Tenure - MonthlyCharges | 0.25 |
| SeniorCitizen - MonthlyCharges | 0.22 |
| SeniorCitizen - TotalCharges | 0.10 |
| SeniorCitizen - Tenure | 0.02 |

The strongest relationship was between `tenure` and `TotalCharges` with a correlation of **0.83**.

The relationship is expected because longer-tenure customers generally accumulate more total charges.

---

# 4. Feature Engineering

The cleaned dataset was prepared for machine learning.

## 4.1 Identifier Removal

`customerID` was removed because it is an identifier rather than a meaningful predictive customer characteristic.

## 4.2 Target Encoding

The target variable was converted to binary form:

```text
No  → 0
Yes → 1
```

## 4.3 Categorical Encoding

The 15 categorical variables were converted into numerical representations using one-hot encoding.

The encoded feature set contained **30 features**.

## 4.4 Train-Test Split

The data was divided using an 80/20 split with stratification.

| Dataset | Records |
|---|---:|
| Training | 5,634 |
| Testing | 1,409 |

The class distribution was preserved:

### Training

- No churn: 4,139
- Churn: 1,495

### Testing

- No churn: 1,035
- Churn: 374

## 4.5 Feature Scaling

The numerical features were standardized for algorithms affected by feature magnitude.

Scaled numerical features:

- `SeniorCitizen`
- `tenure`
- `MonthlyCharges`
- `TotalCharges`

The scaler was fitted using the training data and then applied to the test data to prevent information leakage.

Scaled data was used for:

- Logistic Regression
- KNN

The Decision Tree used the encoded but unscaled feature data.

---

# 5. Classification Models

Three classification algorithms were trained and evaluated using the same held-out test dataset.

## 5.1 Logistic Regression

Logistic Regression achieved:

- Accuracy: **80.70%**
- Precision: **66.04%**
- Recall: **56.15%**
- F1-score: **60.69%**
- ROC-AUC: **84.22%**

For the churn class specifically:

| Metric | Value |
|---|---:|
| Precision | 0.66 |
| Recall | 0.56 |
| F1-score | 0.61 |

---

## 5.2 Decision Tree

Decision Tree achieved:

- Accuracy: **72.60%**
- Precision: **48.38%**
- Recall: **47.86%**
- F1-score: **48.12%**
- ROC-AUC: **64.65%**

For the churn class:

| Metric | Value |
|---|---:|
| Precision | 0.48 |
| Recall | 0.48 |
| F1-score | 0.48 |

---

## 5.3 KNN

KNN achieved:

- Accuracy: **76.65%**
- Precision: **55.97%**
- Recall: **56.42%**
- F1-score: **56.19%**
- ROC-AUC: **79.23%**

For the churn class:

| Metric | Value |
|---|---:|
| Precision | 0.56 |
| Recall | 0.56 |
| F1-score | 0.56 |

---

# 6. Model Comparison

| Model | Accuracy | Precision | Recall | F1 Score | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| Logistic Regression | **0.807** | **0.660** | 0.561 | **0.607** | **0.842** |
| Decision Tree | 0.726 | 0.484 | 0.479 | 0.481 | 0.647 |
| KNN | 0.767 | 0.560 | **0.564** | 0.562 | 0.792 |

## Interpretation

Logistic Regression achieved the highest:

- Accuracy
- Precision
- F1-score
- ROC-AUC

KNN achieved a slightly higher churn recall than Logistic Regression:

- KNN: **56.42%**
- Logistic Regression: **56.15%**

This difference corresponds to one additional correctly detected churner on the test set.

Decision Tree produced lower values across the evaluated metrics on this test split.

The ROC-AUC results also show the separation ability observed during evaluation:

- Logistic Regression: **0.842**
- KNN: **0.792**
- Decision Tree: **0.647**

Based on the complete set of evaluated metrics, Logistic Regression provided the strongest overall performance among the three tested models on this dataset.

---

# 7. Confusion Matrix Analysis

## Logistic Regression

| | Predicted No Churn | Predicted Churn |
|---|---:|---:|
| Actual No Churn | 927 | 108 |
| Actual Churn | 164 | 210 |

The model correctly identified **210 of 374 actual churners** and missed **164**.

## Decision Tree

| | Predicted No Churn | Predicted Churn |
|---|---:|---:|
| Actual No Churn | 844 | 191 |
| Actual Churn | 195 | 179 |

The model correctly identified **179 of 374 actual churners** and missed **195**.

## KNN

| | Predicted No Churn | Predicted Churn |
|---|---:|---:|
| Actual No Churn | 869 | 166 |
| Actual Churn | 163 | 211 |

The model correctly identified **211 of 374 actual churners** and missed **163**.

A key observation is that KNN detected one more churner than Logistic Regression, while Logistic Regression produced fewer false positives and substantially higher ROC-AUC and precision.

---

# 8. Final Model Assessment

The three models demonstrate different performance characteristics.

### Logistic Regression

Provided the strongest overall classification performance in this experiment. It achieved the highest accuracy, precision, F1-score and ROC-AUC.

### KNN

Provided similar churn recall to Logistic Regression and slightly exceeded it on the test set, but had lower precision, F1-score and ROC-AUC.

### Decision Tree

Produced the lowest performance across the main evaluation metrics in this experiment.

Therefore, **Logistic Regression is used as the primary model for generating the final churn predictions in this project**, while the results from KNN and Decision Tree are retained for comparison.

This conclusion is specific to the evaluated dataset, preprocessing procedure, train-test split, and model configurations used in this project.

---

# 9. Final Churn Predictions

Predictions were generated using the Logistic Regression model.

The prediction output contains:

- Original test-set features
- `Actual_Churn`
- `Predicted_Churn`
- `Churn_Probability`

The probability provides a continuous estimate that can be used for further customer-risk analysis rather than relying only on the binary prediction.

The generated prediction file is:

```text
data/processed/churn_predictions.csv
```

---

# 10. Key Findings

The overall analysis identified several important patterns:

1. Approximately **26.5%** of customers in the dataset churned.
2. **Month-to-month contracts** had a much higher observed churn rate than longer contracts.
3. Churned customers generally had **lower tenure**.
4. Churned customers generally had **higher monthly charges**.
5. **Fiber optic** customers showed a higher observed churn rate than DSL and customers without internet service.
6. **Electronic check** users showed the highest observed churn rate among payment methods.
7. Customers without **Tech Support** showed a higher observed churn rate.
8. Customers without **Online Security** showed a higher observed churn rate.
9. `tenure` and `TotalCharges` had a strong positive correlation of **0.83**.
10. Logistic Regression produced the strongest overall model performance among the three tested algorithms.

These findings represent associations observed in the dataset and should not be interpreted as proof of causal relationships.

---

# 11. Limitations

The results should be interpreted within the scope of this experiment.

- The evaluation uses a single stratified train-test split.
- The models were evaluated using the specified baseline configurations rather than extensive hyperparameter optimization.
- Observed relationships in EDA do not establish causation.
- The dataset represents a particular telecom customer population and may not generalize directly to another company or market.
- The prediction threshold was not optimized specifically for a business cost function.
- The model evaluation does not include an independent external validation dataset.

---

# 12. Conclusion

This project developed a complete customer churn classification workflow from raw telecom customer data through exploration, cleaning, EDA, feature engineering and model evaluation.

The final dataset contained **7,043 customer records**, with **30 machine-learning features** after preprocessing.

Three classification approaches were compared:

- Logistic Regression
- Decision Tree
- KNN

Among the evaluated models, Logistic Regression achieved the strongest overall results, with **80.70% accuracy and 0.842 ROC-AUC** on the held-out test set.

The analysis also identified several customer characteristics strongly associated with churn, particularly contract type, tenure, monthly charges, internet service, payment method, technical support and online security.

The project therefore demonstrates how customer-level telecom data can be transformed into a structured classification workflow for identifying patterns associated with customer churn and generating churn-risk predictions.
