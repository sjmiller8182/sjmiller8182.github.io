---
title: "Predicting the Next Recession"
excerpt: "In this analysis, we propose to model the change in GDP for the United States to attempt to predict recessions.<br/><img src='/images/portfolio/gdp_realization.png'>"
collection: portfolio
---

Created by [Stuart Miller](https://github.com/sjmiller8182) and
[Nikhil Gupta](https://github.com/ngupta23).

# Project Overview

Economic recessions are periods of time when an economy shrinks.
These periods of time generally costly to businesses and the populace alike.
Deep recessions can be particularly costly to the populace as business downsizing and
 business failures during recessions generally result in a decrease in available jobs (increasing unemployment).
However, if it was possible to predict a coming recession with some confidence,
 then it may be possible for business and the populace to prepare and mitigate losses.

We propose to model the change in GDP for the United States to attempt to predict recessions.
A working definition of a recession is two consecutive quarters of decrease in GDP.
Thus, we will use a 2-step ahead forecast in evaluating models.
Fifty quarters of historical data will be used for training models to predict the next 2 quarters.
Various ARMA, VAR, and Neural Network models were developed.

The full analysis can be viewed 
[at the project site](https://sjmiller8182.github.io/gdp_prediction/analysis/gdp_prediction_analysis.html).

## Conclusion

* GDP data was very noisy and overall, models were not able to capture variance in this data.
* The univariate model AR(2) performed better than VAR and MLP models.
* An ensemble model appeared to improve the forecasts, but further analysis should be performed (with sliding ASE) since our test dataset consisted of only 2 data points (unfortunately, we did not have enough data to perform this analysis).
* Addition of other exogenous variables with even stronger cross-correlations may improve performance of multivariate models.
* In addition, it may be worthwhile to look for some leading indicators of recession since GDP tends to be a lagging indicator.

## Additional Information

* Project Site: https://sjmiller8182.github.io/gdp_prediction/
* Git Repo: https://github.com/sjmiller8182/gdp_prediction

