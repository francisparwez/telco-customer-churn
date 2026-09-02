# Telco Customer Churn — Project Summary

## Current Stage

The raw data audit and cleaning/preprocessing stages are complete

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

## Next Step

The next stage will focus on feature engineering.
