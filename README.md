# Practical Application II: What Drives the Price of a Car?

This project uses the CRISP-DM process to help a used car dealership understand which vehicle features are associated with higher or lower listing prices. The analysis frames the business question as a supervised regression problem, with `price` as the target variable and vehicle attributes as predictors.

## Author
**Leo Lin**  
Data Reporting and Analytics Consultant, Programming

Notebook: [prompt_2_II.ipynb](prompt_2_II.ipynb)

## Summary of Findings

- Ridge Regression was the best simple model in the test run, with MAE about 4,932 dollars, RMSE about 7,808 dollars, and log-price R-squared about 0.685.
- Newer vehicles and lower-mileage vehicles are generally associated with higher prices.
- Clean title status, body type, drivetrain, fuel type, manufacturer, and condition are useful pricing signals.
- Pickups, trucks, diesel vehicles, 4wd vehicles, and some premium manufacturers tend to show stronger prices.
- Salvage, parts-only, missing-title, older, and high-mileage vehicles should be priced more conservatively.

## Recommendations

- Prioritize inventory with clean titles, newer model years, lower mileage, and high-demand body types such as pickups, trucks, and SUVs.
- Improve listing completeness for condition, drive, size, type, and paint color because missing details reduce confidence in price estimates.
- Use the model as a pricing guide rather than an automatic pricing rule, especially for higher-priced or specialty vehicles where prediction error is larger.

## Data and Modeling Notes

- The notebook cleans likely listing errors, including zero or extreme prices, unrealistic mileage, and missing core fields.
- Visuals are used to summarize price distribution, model year distribution, mileage-price patterns, model comparison, regularization tuning, and actual-vs-predicted prices.
- The modeling approach is intentionally simple: Linear Regression, Ridge Regression, Lasso Regression, cross-validation, and grid search over regularization strength.
- Coefficients are interpreted as associations with price, not proof of causation.

## Methodology

The notebook follows CRISP-DM: business understanding, data understanding, data preparation, modeling, evaluation, and deployment recommendations.

## Next Steps

- Add a cleaned version of vehicle `model` after grouping rare model names.
- Try a larger sample or all cleaned rows if runtime allows.
- Revisit outlier rules if large errors are concentrated in luxury, electric, or specialty vehicles.
