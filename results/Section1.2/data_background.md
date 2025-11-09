# About the Riboflavin data

The dataset comes from the bacterium Bacillus subtilis (used industrially for vitamin B₂ production) and was kindly provided by the company DSM (Switzerland).

It contains n = 71 samples/observations (so 71 rows) and p = 4,088 gene-expression covariates (so 4,088 columns of predictors) + 1 response variable.
The covariates (X matrix) are the logarithms of gene-expression levels for 4,088 genes (so each column is one gene’s log expression).
The response variable (y) is the log-transformed riboflavin production rate (original name: q_RIBFLV).