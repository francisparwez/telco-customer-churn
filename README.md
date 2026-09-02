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

The EDA stage is now complete. The main patterns examined were churn by contract type, tenure, internet service, TechSupport, and monthly charges.

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

## Visuals

The main plots from the EDA are included below.

### Customer Churn

![Customer Churn](images/1_customer_churn.png)

### Customer Tenure

![Customer Tenure](images/2_customer_tenure.png)

### Monthly Charges

![Monthly Charges](images/3_monthly_charges.png)

### Total Charges

![Total Charges](images/4_total_charges.png)

### Contract Type

![Contract Type](images/5_contract_type.png)

### Churn by Contract Type

![Churn by Contract Type](images/6_churn_by_contract_type.png)

### Churn Rate by Contract Type

![Churn Rate by Contract Type](images/7_churn_rate_by_contract_type.png)

### Churn Rate by Tenure Group

![Churn Rate by Tenure Group](images/8_churn_rate_by_tenure_group.png)

### Churn Rate by Internet Service

![Churn Rate by Internet Service](images/9_churn_rate_by_internet_service.png)

### Churn Rate by Tech Support

![Churn Rate by Tech Support](images/10_churn_rate_by_tech_support.png)

### Monthly Charges by Churn

![Monthly Charges by Churn](images/11_monthly_charges_by_churn.png)

### Tenure vs Monthly Charges

![Tenure vs Monthly Charges](images/12_tenure_vs_monthly_charges.png)

## Project Plan

The next stages of the project will cover:

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
├── images/
│   ├── 1_customer_churn.png
│   ├── 2_customer_tenure.png
│   ├── 3_monthly_charges.png
│   ├── 4_total_charges.png
│   ├── 5_contract_type.png
│   ├── 6_churn_by_contract_type.png
│   ├── 7_churn_rate_by_contract_type.png
│   ├── 8_churn_rate_by_tenure_group.png
│   ├── 9_churn_rate_by_internet_service.png
│   ├── 10_churn_rate_by_tech_support.png
│   ├── 11_monthly_charges_by_churn.png
│   └── 12_tenure_vs_monthly_charges.png
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
