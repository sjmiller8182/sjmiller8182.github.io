---
title: "Regression Modeling of Housing Prices in Ames, IA"
excerpt: "A multiple linear regression analysis of housing prices in Ames, IA.<br/><img src='/images/ames_ia_housesbanner.png'>"
collection: portfolio
---

Created by [Stuart Miller](https://github.com/sjmiller8182), 
[Paul Adams](https://github.com/PaulAdams4361), and 
[Chance Robinson](https://github.com/RobinsonCW).

# Project Summary

This was a multiple linear regression analysis of housing prices in Ames, IA (a Kaggle dataset)
for MSDS6371.
The project had two objectives: 

1. Determine if there is a relationship between housing prices and
 the living room area of houses in the Edwards, Northwest Ames, and
 Brookside neighborhoods of Ames, IA.
 Provide an interpretation of the model.
2. Create the most predictive model of housing prices as possible using only OLS regression techniques and
 statistical modeling selection techniques such as forward, backward, and stepwise selection.

## Objective One - Interpretive Modeling

We showed that there is strong evidence of an association between housing prices and
 living room area in the Edwards, Northwest Ames, and Brookside neighborhoods.
We found that the interaction between neighborhood and living room area is significant in explaining the housing prices.
Thus, we modeled the housing prices for these three neighborhoods as a response of the full interaction of living room area and neighborhood.

## Objective Two - Predictive Modeling

We constructed three models using statistical feature selection techniques.
We also created a custom model which was initially produced by eliminating variables suggested by the automatic selection processes,
 visually exploring the data with pairwise scatter plots,
 and adding interaction terms based on graphical analysis and domain knowledge.
We verified that each model met the assumptions of linear regression defore using the model for prediction.
Each model was compaired using the Kaggle score, cross validated PRESS (CV Press),
 and adjusted $R^2$ (shown below).
There is little difference between the scores for the three models.
However, the custom model used substantially fewer features than the other three models.
For this reason, we suggested use of the custom model for regression modeling of the housing prices.
The predictions of the custom model ranked 1948 out of 4565,
 which is reasonably good for only using linear regression on this dataset.

**Model Scores**

| Model | Kaggle.Score | CV.Press | Adjusted.R.Squared |
|-------|--------------|----------|-------------------|
| Custom | 0.13290 | 17.23869 | 0.9303195 |
| Forward Selection | 0.13476 | 17.08652 | 0.9327050 | 
| Backward Selection | 0.13475 | 16.69872 | 0.9327050 | 
| Stepwise Regression | 0.13476 | 17.12312 | 0.9327050 | 

**Custom Model**

```R
log(SalePrice) ~
  BsmtUnfSF + CentralAir + HalfBath + KitchenQual + Neighborhood +
  OverallCond + OverallQual + RoofMatl + `1stFlrSF` + `2ndFlrSF` +
  YearBuilt + MSZoning:Neighborhood + YearBuilt:Neighborhood
```

## Additional Information

* The project repository can be found at [https://github.com/sjmiller8182/RegressionHousingPrices](https://github.com/sjmiller8182/RegressionHousingPrices)
* The project paper can be found [here](https://github.com/sjmiller8182/RegressionHousingPrices/blob/master/analysis/HousePriceRegressionAnalysis.pdf)
* This analysis was conducted in the [R programming language](https://www.r-project.org/about.html).


# References

1. Cock, D. D. (2011). Ames, iowa: Alternative to the boston housing data as an end of semester regression
project. Journal of Statistics Education, 19(3).

2. Kaggle (2016). Ames housing dataset. Data retrieved from the Kaggle website, https://www.kaggle.com/
c/house-prices-advanced-regression-techniques/data.
