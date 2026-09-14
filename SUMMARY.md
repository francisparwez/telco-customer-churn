# Telco Customer Churn — Project Summary

## Project Overview

The raw data audit, cleaning/preprocessing, EDA, feature engineering, baseline modeling, and business recommendations were completed as part of the project.

## Dataset

The dataset contains 7,043 customer records and 21 columns.

It includes customer information, account details, services, billing information, and the `Churn` target.

## Audit Completed

The raw dataset was loaded using Pandas.

The following checks were performed:

- Dataset shape
- Column names
- Data types
- Missing values
- Blank values
- Duplicate rows
- Numerical summaries
- Categorical summaries
- Potential numerical outliers

## Main Data Issue Found

`TotalCharges` is stored as an object even though it contains numeric-looking values. Blank strings were found in this column and were not detected by the initial `isnull()` check.

The column was investigated using numeric conversion before cleaning.

## Cleaning and Preprocessing Completed

The data was prepared for the later machine learning stages.

- `TotalCharges` was converted to numeric.
- The blank `TotalCharges` values were handled after checking the affected records.
- The cleaned data was checked again for missing values.
- `customerID` was removed from the model features.
- `Churn` was separated as the target and converted to 0/1.
- The data was split into training and test sets using stratification.
- Numerical features were handled with median imputation and scaling.
- Categorical features were handled with most-frequent imputation and one-hot encoding.
- The preprocessing steps were fitted on the training data only.
- The fitted preprocessing was then applied to the test data.

## Outlier Check

Potential outliers were checked using the IQR method for the numerical columns.

The values were not removed during the audit because unusual values may represent genuine customers.

## EDA Completed

The main churn patterns were explored using Matplotlib, Seaborn, and Pandas.

The analysis looked at:

- Overall churn distribution
- Tenure distribution
- Monthly and total charges
- Contract type
- Churn rate by contract type
- Churn rate by tenure group
- Churn rate by internet service
- Churn rate by TechSupport
- Monthly charges by churn
- Tenure and monthly charges

Five main business findings were documented from the visual analysis.

## Feature Engineering Completed

Six new features were created from the existing customer data:

- `tenure_group` — groups customers by tenure in months
- `avg_monthly_spend` — average historical monthly charge over the recorded tenure
- `service_count` — number of subscribed services
- `security_support_count` — number of security and support services
- `streaming_services_count` — number of streaming services
- `is_month_to_month` — flags customers with a month-to-month contract

The new features were checked for data types, summary statistics, and missing values. No missing values were found in the new feature columns.

## Modeling Completed

Two baseline classification models were trained and compared:

- Logistic Regression
- Random Forest

The data was split into training and test sets using stratification.

Preprocessing was kept inside the model pipelines so that it was fitted only on the training data during cross-validation.

Five-fold stratified cross-validation was used.

The models were evaluated using precision, recall, F1-score, and ROC-AUC.

### Test Set Results

| Model               | Precision | Recall |     F1 | ROC-AUC |
| ------------------- | --------: | -----: | -----: | ------: |
| Logistic Regression |    0.6532 | 0.5187 | 0.5782 |  0.8421 |
| Random Forest       |    0.6109 | 0.4786 | 0.5367 |  0.8203 |

Logistic Regression performed better than Random Forest across all four metrics on the test set.

The models were used as baseline models, so no extensive hyperparameter tuning was done.

## Business Recommendations

Three main recommendations were identified from the analysis:

1. Encourage month-to-month customers to move to longer contracts.
2. Improve onboarding and support for newer customers.
3. Monitor high-risk service groups such as Fiber optic customers and customers without TechSupport.

These recommendations are based on the EDA findings and baseline model results.

## Adaptive Project — Handling Imbalanced Data & Driving Retention Decisions

### Part 01 — Strengthen Churn Analysis

**Status: ✅ Complete**

The first stage of the adaptive project focused on the class imbalance problem and a deeper review of churn patterns.

The dataset contains 7,043 customers and 1,869 churned customers, giving a churn rate of approximately 26.54%.

A majority-class baseline that predicts every customer as `No Churn` achieves approximately 73.46% accuracy while detecting no churned customers. This shows why accuracy alone is not suitable for the new modelling task.

The analysis revisited churn rates across:

- Contract type
- Tenure groups
- Monthly charge groups
- TechSupport
- Customer characteristics

The strongest differences were found across contract type and tenure.

Month-to-month customers had a churn rate of approximately 42.71%, compared with 11.27% for one-year contracts and 2.83% for two-year contracts.

Customers with 0-12 months of tenure had a churn rate of approximately 47.44%, compared with approximately 9.51% for customers with 49-72 months.

The `TotalCharges` data quality issue was also reconfirmed. Eleven blank values cannot be converted to numeric values, and all affected records have zero tenure.

The results from this stage will guide the class imbalance strategy and model evaluation in the next stage.

### Part 02 — Leakage-Safe Imbalanced Classification Pipeline

**Status: ✅ Complete**

The second stage prepared the churn data for imbalanced machine learning.

Three Logistic Regression approaches were compared:

1. No imbalance adjustment
2. Class weighting
3. SMOTE

The existing preprocessing was kept inside every modelling pipeline.

SMOTE was also kept inside the pipeline so that synthetic samples were created only from the training portion of each cross-validation fold. This prevented validation data from influencing the training process.

Five-fold stratified cross-validation was used with the following metrics:

- Precision
- Recall
- F1-score
- ROC-AUC
- PR-AUC

### Cross-Validation Results

| Strategy        | Precision | Recall |     F1 | ROC-AUC | PR-AUC |
| --------------- | --------: | -----: | -----: | ------: | -----: |
| No Balancing    |    0.6664 | 0.5365 | 0.5938 |  0.8460 | 0.6634 |
| Class Weighting |    0.5183 | 0.7967 | 0.6279 |  0.8457 | 0.6623 |
| SMOTE           |    0.5239 | 0.7846 | 0.6282 |  0.8444 | 0.6608 |

No Balancing achieved the highest PR-AUC by a very small margin.

Class Weighting was selected because it produced much higher recall while keeping ROC-AUC almost unchanged. Since the main purpose of the churn model is to identify customers at risk of leaving, detecting more actual churners is more useful for the next stage.

The test set remained untouched during strategy selection and was used only for the final held-out evaluation of the selected Class Weighting pipeline.

### Current Adaptive Project Progress

1. Strengthen Churn Analysis — ✅ Complete
2. Leakage-Safe Imbalanced Classification Pipeline — ✅ Complete
3. Model Comparison & Hyperparameter Tuning — Planned
4. Threshold Tuning & Model Explanation — Planned
5. Retention Strategy & Executive Report — Planned

## Project Status

The main analysis stages are complete:

- Data audit
- Data cleaning and preprocessing
- EDA
- Feature engineering
- Baseline model training
- Model evaluation
- Business recommendations
