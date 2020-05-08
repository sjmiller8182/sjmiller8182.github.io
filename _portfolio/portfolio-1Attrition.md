---
title: "Attrition Data Analysis"
excerpt: "In this analysis, we present factors related to attrition. Three top factors are presented and relations to other factors are described. Models for attrition and income are presented along with a discussion on performance. Results of the income modeling are interpreted. Finally, some other trends in the data are described..<br/><img src='/images/portfolio/attrition_banner.png'>"
collection: portfolio
---

Created by [Stuart Miller](https://github.com/sjmiller8182).

# Project Summary

In an effort to reduce employee attrition, data has been provided on employees. 
The objective is to analyze the data to determine what factors (if any) correlate to attrition and monthly salary. 
The following will be reported:

* Top 3 factors assocatied with employee turnover
* A model for predicting employee attrition
* A model for predicting monthly salary


## Most Important Attrition Factors

The top three factors associated with attrition that we identified are

* Job Involvement - Lack of involvement
* Work-Life Balance - Lack of work-life balance
* Job Level - Low job levels

## Attrition Model

Attrition was modeled with naive bayes.
The target was to create a model with at least 60% specificity and 60% sensitivity on a 80/20 data split. 
A large number of variables that appeared to be significant for attrition prediction were included. 
Based on the preformance of the model, the goals for sensitivity and specificity were met.

**Model Performance**

| Specificity | Sensitivity |
|--|--|
| 62% | 85% |


**Validation Results**

| Attrition Identified | Attrition Missed |
|--|--|
| 23 | 14 |

## Income Model

Income was modeled with linear regression (OLS).
From data exploration, we suspected that monthly income was correlated to total working years, age, years at the company, years in current role, and years with current manager. 
The following model was used.
The model requires that all categorical variables have the same slope between `MonthlyIncome` and `TotalWorkingYears` becasue no interaction terms were included. 
The categorical variables, `JobLevel` and `JobRole`, only provide a difference in intercept for the regression between MonthlyIncome and TotalWorkingYears.
Estimates for the model parameters are shown in the table below.

$$
\mu \{MonthlyIncome\}=\hat{\beta}_0+\hat{\beta}_1(JobLevel)+\hat{\beta}_2(JobRole)+\hat{\beta}_3(TotalWorkingYears)
$$


From a fit of the model, we found that for an incease in total working years of one year there is an associated increase in mean monthly income of $44.46.
The change in intercept for each job level appears to be significantly different (level 1 was used for reference). 
The change in intercept for each job role appears to be significantly different except for manufacturing director and sales executive. 
There is not sufficent evidence to suggest that the intercepts for manufacturing director and sales executive are significantly different than the reference (healthcare representative).

**Estimate of Model Parameters**

| Variable | Estimate | p-value | 
|----------|----------|---------|
| (Intercept) | 3561.09 | < 2e-16 | 
| Total Working Years | 44.46 | 8.04e-07 | 
| Job Level 2 | 1742.46 | < 2e-16 | 
| Job Level 3 | 4893.21 | < 2e-16 | 
| Job Level 4 | 8191.81 | < 2e-16 | 
| Job Level 5 | 10960.61 | < 2e-16 | 
| Job Role: Human Resources | -984.80 | 0.00084 | 
| Job Role: Laboratory Technician | -1163.75 | 1.75e-08 | 
| Job Role: Manager | 3436.58 | < 2e-16 | 
| Job Role: Manufacturing Director | 153.28 | 0.40279 | 
| Job Role: Research Director | 3562.03 | < 2e-16 | 
| Job Role: Research Scientist | -981.12 | 2.40e-06 | 
| Job Role: Sales Executive | -40.24 | 0.79983 | 
| Job Role: Sales Representative | -1220.67 | 1.71e-06 | 

## Additional Information

* The project repository can be found at [https://github.com/sjmiller8182/AttritionAnalysis](https://github.com/sjmiller8182/AttritionAnalysis).
* [Read the full report.](https://sjmiller8182.github.io/AttritionAnalysis/reports/CaseStudy2DDS.html)
* This analysis was conducted in the [R programming language](https://www.r-project.org/about.html).

