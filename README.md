# XAI-interpretable-churn-model

# Team Name:

## Contributors

## Dataset

Briefly describe the Telco Customer Churn dataset and the churn prediction task.

## Assumption Checks

| Model | Key Assumptions Checked | Evidence | Concern |
|---|---|---|---|
| Linear regression |  |  |  |
| Logistic regression |  |  |  |
| GAM |  Independence, Multicollinearity, Normality, Linearity, Homoscedasticity | Smmoothness and basis functions are two other assumptions which I looked at, since they are specific to GAM. I found that the 'pygam' library handles those assumptions. | Multicollinearity is violated, because the VIFs for some of the predictors are bigger. |

## Model Comparison

| Model | Performance Evidence | Interpretability Strength | Interpretability Weakness |
|---|---|---|---|
| Linear regression |  |  |  |
| Logistic regression |  |  |  |
| GAM | 80% accuracy, but the recall is 54% | It is weak, because interpreting the GAM model is very complex | It is weak, because interpreting the GAM model is very complex |

## Recommendation

Recommended model:

Why this model:

What the company can responsibly conclude:

What the company should not conclude yet:

One next analysis we would run:
