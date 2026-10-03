# Exploratory Data Analysis Report

## 1. Overview

The Exploratory Data Analysis (EDA) was performed on the cleaned Telecom Customer Churn dataset to understand customer characteristics and identify patterns associated with customer churn.

The analysis focused on:

- Overall churn distribution
- Contract type
- Customer tenure
- Monthly charges
- Internet service
- Payment method
- Technical support
- Online security
- Relationships among numerical variables

The EDA is descriptive and identifies associations in the data. It does not establish causal relationships.

---

## 2. Customer Churn Distribution

The target variable `Churn` contains two classes:

- **No:** 5,174 customers (approximately 73.5%)
- **Yes:** 1,869 customers (approximately 26.5%)

The dataset therefore contains a moderate class imbalance, with substantially more customers who did not churn than customers who churned.

This imbalance should be considered during model evaluation. Accuracy alone may not provide a complete picture of model performance, so metrics such as precision, recall, F1-score, and ROC-AUC will also be considered.

---

## 3. Contract Type vs Customer Churn

The analysis shows a strong association between contract type and churn.

| Contract Type | Churn Rate (%) |
|---|---:|
| Month-to-month | 42.71 |
| One year | 11.27 |
| Two year | 2.83 |

Customers with month-to-month contracts have a substantially higher observed churn rate than customers with one-year or two-year contracts.

The count plot also shows that month-to-month customers form a large portion of the dataset and account for most of the observed churn cases.

### Observation

Contract type appears to be an important variable for predicting customer churn.

---

## 4. Tenure vs Customer Churn

The tenure distribution shows a clear difference between churned and retained customers.

The boxplot indicates that:

- Retained customers have a substantially higher median tenure.
- Churned customers are concentrated more strongly toward lower tenure values.
- Customers who remain with the company for longer periods are more frequently represented among the non-churned group.

The distribution also shows a large concentration of customers with very low tenure among the churned group.

### Observation

Customer tenure shows a strong association with churn and is likely to be an important predictive feature.

---

## 5. Monthly Charges vs Customer Churn

The boxplot indicates that churned customers generally have higher monthly charges than retained customers.

The approximate median monthly charge is:

- **Non-churned customers:** around 64
- **Churned customers:** around 80

However, the two distributions overlap considerably.

### Observation

Higher monthly charges are associated with a higher observed churn rate, although monthly charges alone do not clearly separate churned and retained customers.

---

## 6. Internet Service vs Customer Churn

The observed churn rates by internet service are:

| Internet Service | Churn Rate (%) |
|---|---:|
| Fiber optic | 41.89 |
| DSL | 18.96 |
| No internet service | 7.40 |

Fiber optic customers show the highest observed churn rate among the three groups.

Customers without internet service have the lowest observed churn rate.

### Observation

Internet service type shows a substantial association with customer churn and should be considered during feature engineering and modelling.

This analysis does not establish that the type of internet service itself causes churn.

---

## 7. Payment Method vs Customer Churn

The observed churn rates by payment method are:

| Payment Method | Churn Rate (%) |
|---|---:|
| Electronic check | 45.29 |
| Mailed check | 19.11 |
| Bank transfer (automatic) | 16.71 |
| Credit card (automatic) | 15.24 |

Customers using electronic checks have the highest observed churn rate in this analysis.

Customers using automatic payment methods have substantially lower observed churn rates.

### Observation

Payment method is strongly associated with churn and may provide useful predictive information to the classification models.

---

## 8. Tech Support vs Customer Churn

The observed churn rates are:

| Tech Support | Churn Rate (%) |
|---|---:|
| No | 41.64 |
| Yes | 15.17 |
| No internet service | 7.40 |

Customers without technical support have a considerably higher observed churn rate than customers who have technical support.

### Observation

Tech support status appears to be an important variable associated with customer churn.

---

## 9. Online Security vs Customer Churn

The observed churn rates are:

| Online Security | Churn Rate (%) |
|---|---:|
| No | 41.77 |
| Yes | 14.61 |
| No internet service | 7.40 |

Customers without online security show a considerably higher observed churn rate than customers with online security.

### Observation

Online security status shows a strong association with customer churn and may be useful for predictive modelling.

---

## 10. Correlation Between Numerical Variables

The correlation analysis produced the following relationships:

| Variable Pair | Correlation |
|---|---:|
| Tenure - TotalCharges | 0.83 |
| MonthlyCharges - TotalCharges | 0.65 |
| Tenure - MonthlyCharges | 0.25 |
| SeniorCitizen - MonthlyCharges | 0.22 |
| SeniorCitizen - TotalCharges | 0.10 |
| SeniorCitizen - Tenure | 0.02 |

The strongest relationship is between `tenure` and `TotalCharges` with a correlation of **0.83**.

`MonthlyCharges` and `TotalCharges` also show a relatively strong positive correlation of **0.65**.

### Observation

The strong relationship between tenure and total charges is expected because customers who remain subscribed for longer periods generally accumulate more total charges.

These relationships should be considered during feature engineering, particularly when selecting and transforming numerical features.

---

## 11. Overall EDA Findings

The main patterns identified during the analysis are:

1. The dataset contains approximately **26.5% churned customers**, creating moderate class imbalance.
2. **Month-to-month contracts** have a substantially higher observed churn rate than one-year and two-year contracts.
3. **Lower-tenure customers** are more strongly represented among churned customers.
4. Churned customers generally have **higher monthly charges**, although the distributions overlap.
5. **Fiber optic** customers show a higher observed churn rate than DSL and customers without internet service.
6. **Electronic check** users have the highest observed churn rate among the payment methods examined.
7. Customers without **Tech Support** show a higher observed churn rate than customers with Tech Support.
8. Customers without **Online Security** show a higher observed churn rate than customers with Online Security.
9. `tenure` and `TotalCharges` have a strong positive correlation of **0.83**.
10. Several categorical variables show substantial differences in observed churn rates and therefore warrant consideration during feature engineering.

---

## 12. Implications for Feature Engineering

The EDA provides the following direction for the next stage:

- Encode categorical variables so they can be used by the classification algorithms.
- Convert the `Churn` target into a binary representation.
- Retain relevant numerical variables such as `tenure`, `MonthlyCharges`, and `TotalCharges`.
- Consider the strong relationship between `tenure` and `TotalCharges` when preparing the feature set.
- Apply scaling where required, particularly for algorithms sensitive to feature magnitude such as KNN and Logistic Regression.
- Avoid using `customerID` as a predictive feature because it is an identifier rather than a meaningful customer characteristic.
- Perform feature preparation using the training data appropriately to avoid data leakage.

The resulting feature set will be used in the next stage for classification using Logistic Regression, Decision Tree, and KNN.
