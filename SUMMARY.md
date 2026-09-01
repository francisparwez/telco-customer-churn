# Telco Customer Churn — Project Summary

## Current Stage

The project is currently at the raw data audit stage.

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

The column was investigated using numeric conversion to identify values that could not be converted.

The raw data has not been cleaned yet.

## Outlier Check

Potential outliers were checked using the IQR method for the numerical columns.

The values were not removed during the audit because unusual values may represent genuine customers.

## Next Step

The next stage will focus on cleaning and preprocessing the data.

After that, the project will move into exploratory data analysis, feature engineering, and baseline modelling.
