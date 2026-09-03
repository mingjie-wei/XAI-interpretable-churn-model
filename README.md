# XAI-interpretable-churn-model

# Team Name:

## Contributors

## Dataset

Briefly describe the Telco Customer Churn dataset and the churn prediction task.

## Assumption Checks

| Model | Key Assumptions Checked | Evidence | Concern |
|---|---|---|---|
| Linear regression |  |  |  |
| Logistic regression | Independence, Multicollinearity, Linearity, Influential Points | The assumption based on initial inspection of the data (since many tests are suited for logistic regression) is that the observations are independent. The correlation matrix revealed that total charges and tenure are highly correlated with each other. Additionally, VIF revealed that there is high multicollinearity among the service variables. The Box-Tidwell Test revealed that tenure does not have a linear relationship with the log-odds of churn. Cook’s Distance revealed that 242 customers were considered influential, based on the threshold of 4/sample size. Since this is only 3.4% of the sample, this is not enough to be considered a violation. | Since independence was not officially validated, it’s possible that the standard error is underestimated. If the variables with high multicollinearity had not been addressed, the coefficient estimates would likely be unstable and the standard error would be inflated. Since tenure is nonlinear, the coefficients and predictions are less likely to be accurate. In this case, the influential points probably are not a concern. |
| GAM |  Independence, Multicollinearity, Normality, Linearity, Homoscedasticity | Smmoothness and basis functions are two other assumptions which I looked at, since they are specific to GAM. I found that the 'pygam' library handles those assumptions. | Multicollinearity is violated, because the VIFs for some of the predictors are bigger. |

## Model Comparison

| Model | Performance Evidence | Interpretability Strength | Interpretability Weakness |
|---|---|---|---|
| Linear regression |  |  |  |
| Logistic regression | Accuracy: 0.80, Sensitivity: 0.57, Specificity: 0.89, Positive Predictive Value: 0.65, Negative, Predictive Value: 0.85, ROC-AUC: 0.8337, PR-AUC: 0.619 |Compared to many other models, logistic regression provides ways to interpret the coefficients that are intuitive, such as using odds-ratios. | The information that logistic regression provides is only meaningful if the assumptions are met. Since real-world data is not perfect, it is very likely that an assumption will be violated, which makes the results less reliable. |
| GAM | 80% accuracy, but the recall is 54% | It is weak, because interpreting the GAM model is very complex | It is weak, because interpreting the GAM model is very complex |

## Recommendation

Recommended model:

Why this model:

What the company can responsibly conclude:

What the company should not conclude yet:

One next analysis we would run:
