# Practical Application II: What Drives the Price of a Car?

This project uses the CRISP-DM process to analyze the used-car listings dataset and identify features associated with higher or lower prices.

Notebook: [prompt_II.ipynb](prompt_II.ipynb)

## Summary of Findings

- Vehicle age and odometer mileage are major price drivers in the cleaned data, with both showing negative relationships with price.
- Diesel, electric, 4wd, pickup, truck, and clean-title listings tend to have higher median prices than many comparison groups.
- Fuel type, drive type, body type, manufacturer, condition, title status, and transmission should help explain price differences after controlling for age and mileage.
- The notebook compares linear regression, Ridge regression, and Lasso regression using cross-validation and grid search.
- The final recommendations focus on inventory selection, pricing discipline, and improving listing data quality.

## Methodology

The notebook follows CRISP-DM:

1. Business understanding
2. Data understanding
3. Data preparation
4. Modeling
5. Evaluation
6. Deployment recommendations
