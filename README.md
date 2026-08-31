# Telco Customer Churn

This project uses the Telco Customer Churn dataset as the starting point for the next data science task.

The current commit only adds the raw dataset and the initial project README. The analysis and machine learning work will be added in later commits.

## Dataset

The dataset contains 7,043 customer records and 21 columns.

The main fields cover:

- Customer information such as gender, senior citizen status, partner, and dependents
- Account information such as tenure, contract type, payment method, and paperless billing
- Services such as phone service, internet service, online security, online backup, and tech support
- Billing information including monthly charges and total charges
- `Churn`, which is the target column for the later modelling work

The dataset file is:

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
- Summarising the main findings as business recommendations

## Current Repository Structure

```text
telco-customer-churn/
│
├── dataset/
│   └── telco_customer_churn.csv
│
└── README.md
```

More files will be added as the project progresses.

## Tools

The project will use Python, Pandas, NumPy, Matplotlib, Seaborn, and scikit-learn.

## Dataset Note

The raw dataset is being kept in its original form at this stage. Cleaning, feature engineering, and modelling will be handled in the later project steps.
