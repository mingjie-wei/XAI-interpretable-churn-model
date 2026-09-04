# XAI-interpretable-churn-model

## Team Name: dataxplorers

## Contributors

- Mingjie Wei: Linear regression
- Myla Simmons: Logistic regression
- Sebine Scaria: GAM

## Dataset

This project uses the Telco Customer Churn dataset, which contains customer demographics, account information, subscribed services, contract type, payment method, monthly charges, total charges, and whether each customer churned. The prediction task is to model `Churn` as a binary outcome (`Yes`/`No`) and use interpretable models to understand which customer characteristics are associated with higher or lower churn risk.

The original dataset contains 7,043 customers and 21 columns. During cleaning, `TotalCharges` was converted from text to numeric values, and 11 rows with blank `TotalCharges` values were removed.

## Assumption Checks

| Model | Key Assumptions Checked | Evidence | Concern |
|---|---|---|---|
| Linear regression | Linearity, Independent Observations, Homoscedasticity, Normality of Residuals, Multicollinearity, Influential Points | <ul><li><strong>Linearity:</strong> Violated. The residual plot showed visible bands because churn is binary.</li><li><strong>Independent observations:</strong> Likely acceptable. Durbin-Watson was 2.03, suggesting no strong autocorrelation.</li><li><strong>Homoscedasticity:</strong> Violated. The Breusch-Pagan test had a very small p-value (2.268e-52), suggesting non-constant residual variance.</li><li><strong>Normality of residuals:</strong> Violated. The Jarque-Bera test had a very small p-value (3.026e-16), suggesting residuals were not normally distributed.</li><li><strong>Multicollinearity:</strong> Improved but still a concern. Initial VIF results showed perfect or severe multicollinearity from repeated service-status variables and `TotalCharges`; after simplifying service categories and dropping `TotalCharges`, the infinite VIF values were removed, though the highest VIF remained 179.12.</li><li><strong>Influential points:</strong> Present but not dominant. Cook's distance identified 207 rows above the 4/n threshold.</li></ul> | Linear regression is not naturally suited for a binary churn outcome. The residual diagnostics show violations of linearity, homoscedasticity, and residual normality. Multicollinearity improved but still remains because monthly charges and service choices are related, so coefficients should be interpreted as associations rather than causal effects. |
| Logistic regression | Independence, Multicollinearity, Linearity, Influential Points | The assumption based on initial inspection of the data (since many tests are suited for logistic regression) is that the observations are independent. The correlation matrix revealed that total charges and tenure are highly correlated with each other. Additionally, VIF revealed that there is high multicollinearity among the service variables. The Box-Tidwell Test revealed that tenure does not have a linear relationship with the log-odds of churn. Cook’s Distance revealed that 242 customers were considered influential, based on the threshold of 4/sample size. Since this is only 3.4% of the sample, this is not enough to be considered a violation. | Since independence was not officially validated, it’s possible that the standard error is underestimated. If the variables with high multicollinearity had not been addressed, the coefficient estimates would likely be unstable and the standard error would be inflated. Since tenure is nonlinear, the coefficients and predictions are less likely to be accurate. In this case, the influential points probably are not a concern. |
| GAM |  Independence, Multicollinearity, Normality, Linearity, Homoscedasticity | Smoothness and basis functions are two other assumptions which I looked at, since they are specific to GAM. I found that the 'pygam' library handles those assumptions. | Multicollinearity is violated, because the VIFs for some of the predictors are bigger. |

## Model Comparison

| Model | Performance Evidence | Interpretability Strength | Interpretability Weakness |
|---|---|---|---|
| Linear regression | Accuracy at 0.5 threshold: 0.7982, ROC-AUC: 0.8307, MAE: 0.3102, RMSE: 0.3812, R2: 0.2555. For the churn class (`Churn = Yes`), recall was 0.52 and precision was 0.65. | Coefficients are easy to explain as direction and approximate magnitude of change in predicted churn-risk score. This makes linear regression useful as a simple interpretability baseline. | Churn is binary, but linear regression predicts continuous values and can produce invalid probability-like predictions below 0 or above 1. Its residual assumptions are also not well satisfied, so it is weaker as a final churn prediction model. |
| Logistic regression | Accuracy: 0.80, Sensitivity: 0.57, Specificity: 0.89, Positive Predictive Value: 0.65, Negative Predictive Value: 0.85, ROC-AUC: 0.8337, PR-AUC: 0.619 | Compared to many other models, logistic regression provides ways to interpret the coefficients that are intuitive, such as using odds-ratios. | The information that logistic regression provides is only meaningful if the assumptions are met. Since real-world data is not perfect, it is very likely that an assumption will be violated, which makes the results less reliable. |
| GAM | 80% accuracy, but the recall is 54% | It is weak, because interpreting the GAM model is very complex | It is weak, because interpreting the GAM model is very complex |

## Recommendation

**Recommended model:** Logistic regression

**Why this model:**
- Logistic regression is recommended because churn is a binary outcome, and logistic regression is designed for binary classification while still providing interpretable coefficients and odds ratios that help explain how each variable affects predicted churn risk.

- Linear regression is useful as an interpretable baseline, but it predicts continuous values for a `0`/`1` target and showed several residual assumption issues.

- GAM can capture non-linear patterns, but it is more complex to explain and did not provide a large enough performance improvement to justify that added complexity.

- Logistic regression therefore gives the best balance of predictive performance, interpretability, and model appropriateness.

**What the company can responsibly conclude:**
- The company can responsibly use the model results to understand the direction and relative magnitude of relationships between customer characteristics and churn risk.

- Positive churn-risk signals include fiber optic internet service, electronic check payment, and month-to-month contracts.

- Negative churn-risk signals include longer tenure, one-year or two-year contracts, and no internet service.

- Across the analyses, the most dominant churn-related feature groups were contract type, tenure, internet service type, payment method, and charges.

**What the company should not conclude yet:**
- The company should not treat these relationships as causal effects. The assumption checks show remaining concerns, including multicollinearity among service and charge variables and possible non-linear patterns such as tenure.

- Because of these limitations, the company should be careful about saying that changing one feature would directly cause churn to increase or decrease.

**One next analysis we would run:**
- A useful next step would be to test models that can capture non-linear relationships and interactions more naturally, such as tree-based models.

- These could be compared with logistic regression to see whether predictive performance improves while still using interpretability tools to explain the results.
