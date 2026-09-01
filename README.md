# Telco Customer Churn

This project looks at customer churn in a telecom dataset using Python and Pandas.

The goal is to work through the data step by step, starting with a raw data audit and later moving into EDA, feature engineering, and baseline machine learning models.

## Current Progress

The first stage of the project is the raw data audit.

The dataset was loaded with Pandas and checked for:

- Missing values
- Duplicate rows
- Data type issues
- Blank values
- Numerical outliers

One of the main issues found during the audit is that `TotalCharges` is stored as an object even though it contains numeric-looking values.

The raw dataset has 7,043 rows and 21 columns.

## Dataset

The dataset contains customer information, account details, subscribed services, billing information, and the `Churn` target.

The main types of information include:

- Customer information such as gender, senior citizen status, partner, and dependents
- Account information such as tenure, contract type, payment method, and paperless billing
- Services such as phone service, internet service, online security, online backup, and tech support
- Billing information including monthly charges and total charges
- `Churn`, which is the target column for the later modelling work

The raw dataset is stored at:

```text
dataset/telco_customer_churn.csv
```

## Project Plan

The next stages of the project will cover:

- Checking the raw data for missing values, duplicates, type issues, and outliers
- Cleaning and preprocessing the data
- Exploring churn patterns with visualizations
- Creating new features that may help explain customer churn
- Comparing baseline classification models
- Summarizing the main findings as business recommendations

## Project Structure

```text
telco-customer-churn/
│
├── dataset/
│   └── telco_customer_churn.csv
│
├── Telco_Churn.ipynb
├── README.md
├── SUMMARY.md
└── requirements.txt
```

More files will be added as the project progresses.

## Tools

The project uses Python, Pandas, NumPy, Matplotlib, Seaborn, scikit-learn, and Jupyter.

## Dataset Note

The raw dataset is being kept in its original form at this stage. Cleaning, feature engineering, and modelling will be handled in the later project steps.
