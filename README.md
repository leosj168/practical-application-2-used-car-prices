# Practical Application II: What Drives the Price of a Car?

This project follows the CRISP-DM process to help used-car dealers identify listing characteristics associated with price. I frame the work as a supervised regression problem with `price` as the target and vehicle details available to a dealer as predictors.

## Author

**Leo Lin**  
Data Reporting and Analytics Consultant, Programming

Notebook: [prompt_2_II.ipynb](prompt_2_II.ipynb)

## Executive Summary

I recommend tuned **Lasso Regression with `alpha=0.00005`** as the best observed model for this analysis. It has the lowest mean cross-validated MAE on log price at `0.301235`, compared with `0.301298` for tuned Ridge. This difference is very small and falls within the fold-to-fold variation, so the result supports Lasso for this sample but does not show that Lasso is meaningfully better than Ridge in general.

On the held-out test set, tuned Lasso achieved:

- MAE: about **$4,739**
- RMSE: about **$7,598**
- Log-price R-squared: about **0.698**

A simple median-price baseline had MAE of about **$11,058** and RMSE of about **$14,474**. Lasso therefore adds useful pricing information, but the remaining error is still large enough that dealer judgment should remain part of every final pricing decision.

## Business Question and Goal

The dealership wants to know which used-car characteristics are associated with higher or lower prices. My goal is to identify dependable signals that can help dealers fine-tune inventory, review asking prices, and improve listing quality. The model is a pricing-support tool, not an automatic pricing rule.

## Data Preparation and Feature Decisions

The raw listings include zero or extreme prices, unrealistic mileage, missing vehicle details, and many rare categorical values. I keep the original data unchanged and create a separate modeling table that:

1. Removes identifiers such as `id` and `VIN`.
2. Filters prices to $500-$100,000, model years to 1980-2022, and odometer readings to 100-300,000 miles.
3. Creates vehicle age using 2022, the newest model year found in the cleaned data.
4. Applies `log1p` to price and odometer to reduce skew.
5. Fills missing numeric values with the training-fold median and categorical values with `unknown`.
6. Standardizes model names and groups low-count categories.

The final feature set includes vehicle age, raw and log mileage, manufacturer, cleaned model name, condition, cylinders, fuel, title status, transmission, drivetrain, size, body type, paint color, and state. Raw and log mileage are both retained so a linear model can represent a curved mileage relationship.

I did not assume that the high-detail `model` field would help. A cross-validated feature test showed that adding `model_clean` reduced mean MAE on log price from about `0.312` to `0.301`, an improvement of roughly **3.6%**, so I retained it.

## Modeling and Selection

I used methods covered in the program and kept the decision process easy to audit:

1. Linear Regression established an unregularized baseline.
2. Ridge tested whether regularization could stabilize related predictors while retaining all coefficients.
3. Lasso tested coefficient shrinking in the one-hot encoded feature set.
4. Three-fold cross-validation compared feature sets and models using MAE on log price.
5. Grid search tuned only `alpha` for Ridge and Lasso.
6. The model with the lowest mean cross-validated MAE was selected before the held-out test set was evaluated.

Default Lasso performed poorly because `alpha=1` shrank the coefficients too strongly. The expanded grid tested much smaller values and confirmed an interior minimum at `alpha=0.00005`; values both above and below it were included. Tuned Lasso retained 196 of 210 encoded coefficients, so it provides mild shrinking rather than a highly sparse model.

## Primary Findings

**Cleaned model names add value.** The feature test shows that grouped model names improve prediction. Corvette, Wrangler, Silverado, 4Runner, Tacoma, and F-Series truck indicators appear among the stronger positive coefficients in this sample.

**Age and mileage remain core pricing signals.** The descriptive plots show that newer vehicles and lower odometer readings generally align with higher prices. Older or high-mileage vehicles should receive a more conservative review unless other value signals support the asking price.

**Title status is an important risk signal.** Median prices are much lower for salvage, missing-title, and parts-only listings than for clean-title vehicles.

**Body type and market position matter.** Pickups and trucks have stronger median prices than many passenger-car body types, and several premium manufacturers show positive model coefficients.

**Listing completeness affects confidence.** Condition, drive, size, type, and paint color have substantial missing rates. Complete listing details give dealers more information for both model-supported pricing and manual review.

These findings are associations, not proof that changing one characteristic causes a specific price change. One-hot encoded categorical coefficients are interpreted relative to an omitted reference category and alongside the median-price summaries.

## Recommendations for Used-Car Dealers

- Use the Lasso estimate as a consistent starting point for pricing review, not as the final asking price.
- Prioritize complete, clean-title listings with lower mileage and stronger body/model combinations.
- Review pickups, trucks, and high-value specialty models carefully because their price patterns differ from common passenger vehicles.
- Price older, high-mileage, salvage, parts-only, or missing-title vehicles more conservatively unless inspection and local demand support a higher price.
- Apply additional judgment to expensive vehicles because the actual-versus-predicted plot shows wider errors at the high end.
- Keep Ridge as a reasonable alternative if future samples show equal performance and retaining stable coefficients is more important than Lasso shrinking.

## Limitations

- The analysis uses a reproducible 50,000-row sample to keep runtime manageable.
- Simple model-name cleaning does not fully separate trims, abbreviations, or overlapping names.
- Prediction errors are larger for expensive vehicles.
- Luxury, electric, and specialty vehicles may follow pricing patterns that are not fully represented by one general model.
- The results describe associations in the listings and should not be interpreted as causal effects.

## Next Steps

1. Refine model and trim grouping, then retest the feature with the same cross-validation process.
2. Review the largest residuals to identify vehicle segments that may need separate treatment.
3. Repeat the close Ridge-versus-Lasso comparison with more folds or more rows before operational use.
4. Revisit filtering rules with dealer input and test the final process on newer listing data.
