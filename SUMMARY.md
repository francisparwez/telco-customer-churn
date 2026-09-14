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

### Part 03 — Model Comparison & Hyperparameter Tuning

**Status: ✅ Complete**

Three classification models were compared using the Class Weighting strategy selected in Part 02:

1. Logistic Regression
2. Random Forest
3. XGBoost

Each model used the existing preprocessing pipeline.

Five-fold stratified cross-validation was used for hyperparameter tuning.

PR-AUC was used as the primary tuning metric because the churn class is imbalanced. Precision, recall, F1-score, ROC-AUC, and PR-AUC were also used to compare the tuned models.

The models were ranked using their cross-validated PR-AUC results. The best-performing model was then evaluated once on the untouched test set. XGBoost had the highest cross-validated PR-AUC and was selected for the next stage.

### Cross-Validation Results

| Model               | Precision | Recall |     F1 | ROC-AUC | PR-AUC |
| ------------------- | --------: | -----: | -----: | ------: | -----: |
| XGBoost             |    0.5249 | 0.7987 | 0.6334 |  0.8480 | 0.6671 |
| Logistic Regression |    0.5183 | 0.7967 | 0.6279 |  0.8457 | 0.6623 |
| Random Forest       |    0.5621 | 0.7124 | 0.6283 |  0.8453 | 0.6602 |

XGBoost had the highest cross-validated PR-AUC and was selected as the best model.

The best XGBoost settings were:

- `learning_rate`: 0.05
- `max_depth`: 3
- `n_estimators`: 200
- `subsample`: 0.8

### Test Set Results

| Model   | Precision | Recall |     F1 | ROC-AUC | PR-AUC |
| ------- | --------: | -----: | -----: | ------: | -----: |
| XGBoost |    0.5157 | 0.7914 | 0.6245 |  0.8474 | 0.6599 |

The test set was kept separate during hyperparameter tuning and model selection and was used only for the final evaluation of the selected model.

### Part 04 — Decision Threshold & Model Explanation

**Status: ✅ Complete**

The fourth stage focused on choosing a practical classification threshold for the selected XGBoost model.

Instead of automatically using the default 0.50 threshold, out-of-fold training predictions were used to evaluate a range of thresholds.

A working business cost assumption was used:

- Missing a customer who churns = cost of 3
- Incorrectly targeting a customer who stays = cost of 1

The selected threshold was the threshold with the lowest estimated business cost under this assumption.

### Threshold Comparison

| Threshold | Precision | Recall |     F1 | False Positives | False Negatives | Business Cost |
| --------: | --------: | -----: | -----: | --------------: | --------------: | ------------: |
|      0.50 |    0.5248 | 0.7987 | 0.6334 |            1081 |             301 |          1984 |
|      0.48 |    0.5157 | 0.8140 | 0.6314 |            1143 |             278 |          1977 |

### Final Test Result at Selected Threshold

| Threshold | Precision | Recall |     F1 |
| --------: | --------: | -----: | -----: |
|      0.48 |    0.5119 | 0.8075 | 0.6266 |

The ROC and precision-recall curves were used to examine model behaviour across thresholds.

The selected threshold was 0.48. Compared with the default 0.50 threshold, recall increased from 0.7987 to 0.8140 while the estimated business cost decreased from 1984 to 1977 under the working cost assumption.

XGBoost feature importance identified `is_month_to_month` as the strongest model feature, followed by `InternetService_Fiber optic`, `OnlineSecurity_No`, `TechSupport_No`, and `StreamingMovies_Yes`.

These importance values describe model behaviour and should not be interpreted as proof of causation.

These results will be used in the final retention strategy and executive summary.

### Part 05 — Retention Strategy & Executive Report

**Status: ✅ Complete**

The final stage translated the model results into a practical retention strategy.

The final model was XGBoost, selected because it achieved the highest cross-validated PR-AUC of 0.6671.

Class Weighting was used to handle the imbalanced churn target.

The selected decision threshold was 0.48.

### Final Model Results

| Measure                |          Result |
| ---------------------- | --------------: |
| Model                  |         XGBoost |
| Imbalance strategy     | Class Weighting |
| Cross-validated PR-AUC |          0.6671 |
| Test ROC-AUC           |          0.8474 |
| Test PR-AUC            |          0.6599 |
| Selected threshold     |            0.48 |
| Test Precision at 0.48 |          0.5119 |
| Test Recall at 0.48    |          0.8075 |
| Test F1 at 0.48        |          0.6266 |

The model is intended to prioritise higher-risk customers for retention outreach.

The strongest model features were:

- `is_month_to_month`
- `InternetService_Fiber optic`
- `OnlineSecurity_No`
- `TechSupport_No`
- `StreamingMovies_Yes`

The main retention recommendations are:

1. Prioritise customers above the 0.48 churn-probability threshold.
2. Encourage month-to-month customers to consider longer contracts.
3. Improve onboarding and support for newer customers.
4. Review support and service options for customers without TechSupport or OnlineSecurity.
5. Monitor Fiber optic customers for service and pricing concerns.
6. Use targeted retention actions instead of automatically giving discounts to every high-risk customer.

The expected trade-off is that a lower threshold captures more likely churners but also increases unnecessary outreach.

The 3-to-1 business cost assumption used during threshold selection is illustrative and should be replaced with actual customer value and retention campaign costs before financial decisions are made.

### Executive Summary

The completed project provides an end-to-end churn modelling workflow from data audit and EDA through imbalance handling, model tuning, threshold selection, feature importance, and business recommendations.

The final recommendation is to use XGBoost with Class Weighting and a 0.48 decision threshold as the starting point for a controlled retention pilot.

The model should be used to prioritise customers, while the final retention action should be based on customer context and the measured effectiveness of each intervention.

### Current Adaptive Project Progress

1. Strengthen Churn Analysis — ✅ Complete
2. Leakage-Safe Imbalanced Classification Pipeline — ✅ Complete
3. Model Comparison & Hyperparameter Tuning — ✅ Complete
4. Threshold Tuning & Model Explanation — ✅ Complete
5. Retention Strategy & Executive Report — ✅ Complete

## Project Status

The full Telco Customer Churn project is complete.

The project covers:

- Data audit
- Data cleaning and preprocessing
- Exploratory data analysis
- Feature engineering
- Baseline modelling
- Class imbalance handling
- Model comparison
- Hyperparameter tuning
- Decision threshold tuning
- Feature importance
- Retention decision strategy
- Executive reporting
