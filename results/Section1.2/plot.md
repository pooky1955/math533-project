# Plot

Bars: 
The fitted coefficient of each model for the same subset of features (genes).

- Blue (RidgeCV): L2-penalized regression → shrinks many coefficients toward 0 but rarely sets them exactly to 0.

- Orange (ElasticNetCV): mix of L1+L2 → keeps fewer features with larger magnitudes; zeros the rest.

- Green (“LassoCV”): pure L1 → sparsest; keeps only a handful with the largest magnitudes.

Black dots (at y=0): 
These are the stability-selected features—variables that were repeatedly selected across bootstrap subsamples. They’re plotted at 0 just to mark the features; they’re not the “true value”.


# How to read a single tick (one gene)

For each gene on the x-axis, compare the three bars:

Sign (above/below 0): direction of association with riboflavin production.

- Above 0 → higher expression predicts higher production (all else equal).

- Below 0 → higher expression predicts lower production.

Magnitude: strength of that association. 
Larger |coefficient| = stronger effect (conditional on the linear model).

Cross-model agreement: 
- If Lasso and ENet agree on sign and both have sizable bars, that gene is a robust signal
- If Ridge is near 0 while Lasso/ENet are non-zero, that hints at sparse signal amid collinearity.


# Understanding the interpretation of the graph (Examples)

Negative coefficient example: “YOAB_at”
The bars above/below 0 show how each model weighted that gene. A negative bar means higher expression of that gene → lower riboflavin production (given the model’s fit).

Positive coefficient example: “SPOVAA_at”
It shows a positive bar → higher expression → higher riboflavin production according to the model.


# Interpretation of the plot

Overall, this plot suggests that the riboflavin dataset contains a sparse, collinear signal: only a small subset of genes carry meaningful weight, and those genes are highly correlated with others. 

You see this because Lasso and Elastic Net assign clear, non-zero coefficients to a handful of features while Ridge spreads tiny weights across many, a classic pattern when p > n and correlations are strong. The signs also tell a coherent story—most selected genes have negative coefficients (higher expression predicting lower riboflavin production), with a couple of positive outliers indicating potential enhancers. 

The magnitudes are modest, consistent with noisy biological measurements and with your CV results where Lasso achieves the best RMSE/R^2 using only ~40 non-zeros, while Ridge is fully dense yet predicts worse. By combining everything together, the evidence points to a few robust transcripts driving predictive performance, with Elastic Net acting as a stabilizing middle ground in the presence of multicollinearity.

# Some definitions
- Repeated subsampling: Instead of fitting the model once on the full dataset, you repeatedly take random subsets of the rows (observations) and refit the model on each subset. This is the core concept of stability selection: it shows which features are chosen consistently across many resampled versions of the data, rather than relying on a single, potentially unstable split.

- Elastic Net regularization: Penalty used in regression that combines Lasso (L1) and Ridge (L2) penalties in one objective. It gives both sparsity (like Lasso) and stability under correlation (like Ridge).


- Random feature weighting: On each subsample, you multiply each column of X by a small random factor before fitting the model. This reinforces the stability selection as it tests features under slightly different “conditions” and highlights those that remain important despite these perturbations.

# Feature Selection
A previous analysis was conducted using the riboflavin dataset by Städler et al. (2010) and in the textbook Statistics for High-Dimensional Data, the authors worked with the full high-dimensional setting of 4088 gene-expression covariates. The reason their figure displays only 20 genes is that they applied the FMRLasso (finite mixture of regression with an L1 penalty) and then selected the top 20 most influential genes based on the sum of the absolute estimated coefficients across the three mixture components.

In our analysis, we employ an enhanced version of stability selection, incorporating repeated subsampling, Elastic Net regularization, and random feature weighting, which is more suitable for real-world high-dimensional genomic data. This approach more effectively handles strong gene-gene correlations, produces more stable and reproducible feature sets than FMRLasso alone, and identifies genes that consistently contribute to prediction across resampled datasets. Thus, while our visualization mimics the style of the original riboflavin figure, the underlying feature-selection procedure is more robust for modern high-dimensional data analysis.


