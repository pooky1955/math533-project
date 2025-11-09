# Table

1. RMSE (CV): Root Mean Squared Error computed with cross-validated predictions.

Interpretation: Average size of prediction mistakes in the units of y. The lower that y is, the better it is.

This corresponds to prediction error. Since for real data, we do not know the true value of beta, so we focus on how well the model predicts unseen data


2. R^2(CV): Cross-validated coefficient of determination (variance explained).

Interpretation: Fraction of the variance in y explained by the model (higher is better).

This is also a prediction-quality metric (similar to “prediction error”), just on a normalized scale instead of units of y.


3. Sparcity: Number of non-zero coefficients in the fitted model on the full dataset.


# Interpretation of the table

LassoCV: best RMSE (0.471) and best R^2 (~0.70) with only 41 features ⇒ signal looks sparse; L1 helps.

ElasticNetCV: slightly worse predictive metrics than Lasso but still strong; keeps more features (161)—consistent with correlated genes where ENet stabilizes selection.

RidgeCV: worst of the three on prediction and fully dense (4088 non-zeros) ⇒ typical when p > n and the true signal is concentrated in a subset.