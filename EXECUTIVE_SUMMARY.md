# Telco Customer Churn — Executive Summary

## Objective

The goal of this project was to identify telecom customers who are likely to churn and use those predictions to improve retention decisions.

The dataset contains 7,043 customers and approximately 26.54% of customers churned.

Because churn is an imbalanced target, accuracy alone was not used as the main measure of model quality.

## Final Model

Three models were compared:

- Logistic Regression
- Random Forest
- XGBoost

XGBoost was selected because it achieved the highest cross-validated PR-AUC of 0.6671.

Class Weighting was selected as the imbalance strategy because it substantially improved recall while keeping ROC-AUC almost unchanged.

### Final Test Performance

| Metric    | Result |
| --------- | -----: |
| ROC-AUC   | 0.8474 |
| PR-AUC    | 0.6599 |
| Precision | 0.5157 |
| Recall    | 0.7914 |
| F1-score  | 0.6245 |

## Decision Threshold

The default 0.50 threshold was replaced with a threshold selected using out-of-fold training predictions.

A working business assumption treated a missed churner as three times more costly than an unnecessary retention offer.

The selected threshold was 0.48.

### Test Performance at 0.48

| Metric    | Result |
| --------- | -----: |
| Precision | 0.5119 |
| Recall    | 0.8075 |
| F1-score  | 0.6266 |

The lower threshold identifies more likely churners, but it also increases the number of customers who may receive outreach even though they would have stayed.

This is an expected trade-off in retention modelling.

## Retention Outreach Trade-Off

At the selected 0.48 threshold, the model would have selected 590 of the 1,409 test customers for retention outreach, or approximately 41.87% of the customers.

Among those selected customers, 302 were actual churners and 288 would have stayed anyway. The model missed 72 actual churners.

This means the model captured approximately 80.75% of actual churners while accepting that some customers receiving outreach would not have churned.

Approximately 48.81% of the customers selected for outreach would have stayed based on the test-set results.

This trade-off is expected in churn retention modelling. The company should therefore use the model to prioritise customers and then choose the most appropriate and cost-effective retention action rather than automatically applying a discount to every flagged customer.

## Main Churn Drivers

The strongest model features were:

1. `is_month_to_month`
2. `InternetService_Fiber optic`
3. `OnlineSecurity_No`
4. `TechSupport_No`
5. `StreamingMovies_Yes`

The model also supports the broader EDA finding that contract type and customer tenure are strongly associated with churn.

## Recommended Retention Strategy

### 1. Prioritise high-risk customers

Use the 0.48 probability threshold to create a high-risk retention list.

Customers above this threshold should receive more attention than lower-risk customers.

### 2. Focus on month-to-month customers

Month-to-month customers showed substantially higher churn than customers with longer contracts.

The company should test incentives and service improvements that encourage customers to move to longer-term contracts.

### 3. Improve the first-year experience

Customers with shorter tenure showed much higher churn.

Better onboarding, early support, and proactive follow-up could help reduce early cancellations.

### 4. Review service-related risk

Customers without TechSupport, customers without OnlineSecurity, and Fiber optic customers should be reviewed for service quality, support needs, and pricing concerns.

### 5. Avoid unnecessary discounts

The model should identify who deserves attention, not automatically trigger the most expensive retention offer.

A lower-cost action such as proactive support or a service review may be more appropriate for some customers.

## Business Use

The model should be used as a decision-support tool.

A practical process would be:

**Score customers → identify high-risk customers → review customer context → choose an appropriate retention action → measure the result**

The current business cost assumption is illustrative.

Before the model is used for financial decisions, the company should replace it with actual customer lifetime value, retention campaign cost, and expected retention benefit.

## Final Recommendation

Start with a controlled retention pilot using XGBoost, Class Weighting, and the 0.48 decision threshold.

Prioritise high-risk customers, tailor the outreach to the customer's situation, and measure which retention actions actually reduce churn.

The model should then be monitored and retrained as customer behaviour changes.
