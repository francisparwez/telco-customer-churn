# Telco Customer Churn

This project looks at customer churn in a telecom dataset using Python and Pandas.

The goal is to work through the data step by step, starting with a raw data audit, then cleaning and preprocessing the data before moving into EDA, feature engineering, and baseline machine learning models.

## Current Progress

The raw data audit and cleaning/preprocessing stages are complete.

The audit checked for:

- Missing values
- Duplicate rows
- Data type issues
- Blank values
- Numerical outliers

One of the main issues found was that `TotalCharges` was stored as an object even though it contained numeric-looking values.

The data was then cleaned and prepared for machine learning by:

- Converting `TotalCharges` to numeric
- Handling the blank `TotalCharges` values
- Checking the cleaned data for missing values
- Removing `customerID` from the model features
- Separating `Churn` as the target
- Converting `Churn` to 0 and 1
- Splitting the data into training and test sets
- Using stratification to keep the churn proportions similar
- Imputing missing numerical and categorical values
- One-hot encoding categorical variables
- Scaling numerical variables
- Fitting the preprocessing steps on the training data only

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

The raw dataset is kept in its original form. Cleaning and preprocessing are done on a working copy in the notebook.
