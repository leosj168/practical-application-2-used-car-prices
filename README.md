# Practical Application II: What Drives the Price of a Car?

This project follows the CRISP-DM process to help used-car dealers understand which listing features are most associated with price. I frame the business question as a supervised regression problem: `price` is the target, and vehicle details are the predictors.

## Author
**Leo Lin**  
Data Reporting and Analytics Consultant, Programming

Notebook: [prompt_2_II.ipynb](prompt_2_II.ipynb)

## Executive Summary

The final model is tuned Lasso Regression with a cleaned version of the raw `model` field included. Testing `model_clean` was an important improvement: cross-validated MAE on log price improved from about `0.312` with the base feature set to about `0.301` after adding grouped model names. After expanding the alpha grid, Lasso slightly outperformed Ridge, although the two models were very close.

On the held-out test set, the final Lasso model had:

- MAE: about **$4,739**
- RMSE: about **$7,598**
- Log-price R-squared: about **0.698**

This means the model is useful for pricing guidance and inventory review, but the typical error is still large enough that dealer judgment should remain part of the pricing process.

## Business Question

The dealership wants to know which used-car characteristics tend to move prices higher or lower. My goal is not to build an automatic pricing tool. My goal is to identify pricing signals that can help dealers fine-tune inventory decisions, improve listings, and review pricing more consistently.

## Data Preparation and Feature Decisions

I cleaned the data before modeling because the raw listings included zero or extreme prices, unrealistic mileage, missing vehicle details, and many rare categorical values. I kept the original data unchanged and created a separate modeling table.

The final feature set uses information a dealer would normally know when pricing a vehicle: vehicle age, odometer, log odometer, manufacturer, cleaned model name, condition, cylinders, fuel, title status, transmission, drivetrain, size, body type, paint color, and state.

I excluded identifiers such as `id` and `VIN` because they do not explain price in a useful business way. I also dropped `region` because state already gives a simpler location signal for this starter model.

## Modeling Approach

I used a simple modeling path covered in the program:

1. Test whether adding `model_clean` improves cross-validated MAE.
2. Compare Linear Regression, Ridge Regression, and Lasso Regression.
3. Use grid search to tune regularization strength (`alpha`) for Ridge and Lasso.
4. Select the model with the best cross-validated MAE.
5. Use the held-out test set only for final evaluation.

Lasso Regression was selected because the expanded alpha grid found a very small alpha that slightly improved cross-validated MAE. Ridge was nearly tied, so the practical takeaway is that very light regularization worked best for this starter model.

## Primary Findings

**Model names matter.** After grouping rare model names, `model_clean` improved cross-validated performance. Specific models such as Corvette, Wrangler, Silverado, 4Runner, and F-Series trucks showed stronger positive price associations in the final coefficient table.

**Mileage and age remain core price signals.** Newer vehicles and lower odometer readings are generally associated with higher prices. This supports a more conservative pricing review for older or high-mileage vehicles unless other features justify a higher price.

**Title status matters.** Clean title vehicles show stronger prices than salvage, missing-title, or parts-only vehicles. These title issues should be treated as clear pricing risk signals.

**Body type and market positioning matter.** Pickups and trucks show stronger pricing patterns than many passenger-car body types. Premium manufacturers also tend to show higher predicted prices.

**Listing completeness matters.** Missing condition, drive, size, type, model, or paint color reduces pricing confidence. Better listings should help dealers make stronger use of model-supported pricing.

## Recommendations for Dealers

- Use the model as a pricing support tool, not an automatic pricing rule.
- Prioritize clean-title, lower-mileage inventory with stronger body/model combinations.
- Review pickups, trucks, and high-value specialty models carefully because they show stronger price associations.
- Price older, high-mileage, salvage, parts-only, or missing-title vehicles more conservatively unless other value signals support a higher price.
- Improve listing completeness before relying on model-supported pricing.
- Treat higher-priced vehicles with extra care because the actual-vs-predicted plot shows wider errors at the high end.

## Limitations

- The model estimates associations, not proof that one feature causes a price change.
- The `model_clean` field is useful, but it still uses simple text cleanup and rare-category grouping.
- The model uses a 50,000-row sample for runtime, so future work should test whether the results hold on more rows.
- Expensive, luxury, electric, or specialty vehicles may need additional review because prediction errors are larger for high-priced listings.

## Next Steps

1. Refine `model_clean` for trims, abbreviations, and overlapping model names.
2. Try a larger sample or all cleaned rows if runtime allows.
3. Confirm the small-alpha Lasso result with more cross-validation folds or a larger modeling sample.
4. Review large-error records to see whether luxury, electric, or specialty vehicles need separate treatment.
5. Revisit outlier rules with business input before using the model for pricing decisions.
