# Telco Customer Churn — Project Summary

## Current Stage

The raw data audit, cleaning/preprocessing, EDA, and feature engineering stages are complete.

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

## Next Step

The next stage is to check how the engineered features relate to churn, then rebuild the train/test preprocessing so the new features are included before moving into model training.
