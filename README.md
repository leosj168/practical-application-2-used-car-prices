# Practical Application II: What Drives the Price of a Car?

This project uses the CRISP-DM process to analyze which vehicle features are associated with higher or lower used-car listing prices. I frame the business question as a supervised regression problem where `price` is the target variable and vehicle attributes are the predictors.

## Author
**Leo Lin**  
Data Reporting and Analytics Consultant, Programming

Notebook: [prompt_2_II.ipynb](prompt_2_II.ipynb)

## Summary of Findings

- The best simple model was Ridge Regression, with MAE about 4,932 dollars, RMSE about 7,808 dollars, and log-price R-squared about 0.685.
- Newer vehicles and lower-mileage vehicles are generally associated with higher prices.
- Clean title status, body type, drivetrain, fuel type, manufacturer, and condition are useful pricing signals.
- Pickups, trucks, diesel vehicles, 4wd vehicles, and some premium manufacturers tend to show stronger prices.
- Salvage, parts-only, missing-title, older, and high-mileage vehicles should be priced more conservatively.

## Recommendations

- Prioritize inventory with clean titles, newer model years, lower mileage, and stronger-performing body types such as pickups and trucks.
- Improve listing completeness for condition, drive, size, type, and paint color because missing details reduce confidence in price estimates.
- Use the model as a pricing guide rather than an automatic pricing rule, especially for higher-priced or specialty vehicles where prediction error is larger.

## Data and Modeling Notes

- The analysis cleans likely listing errors, including zero or extreme prices, unrealistic mileage, and missing core fields.
- Visuals summarize price distribution, model year distribution, mileage-price patterns, categorical median prices, model comparison, regularization tuning, and actual-vs-predicted prices.
- The modeling approach is intentionally simple: Linear Regression, Ridge Regression, Lasso Regression, cross-validation, and grid search over regularization strength.
- Coefficients are interpreted as associations with price, not proof of causation.

## Methodology

This project follows CRISP-DM: business understanding, data understanding, data preparation, modeling, evaluation, and deployment recommendations.

## Next Steps

- Clean and group the vehicle `model` column before adding it to a future model.
- Try a larger sample or all cleaned rows if runtime allows.
- Revisit outlier rules if large errors are concentrated in luxury, electric, or specialty vehicles.
