---
title: "Attrition Data Analysis"
excerpt: "In this analysis, we present findings from loan default data. We provide a model for predicting loan default status and interpretable customer segmentation.<br/><img src='/images/portfolio/attrition_banner.png'>"
collection: portfolio
---

Created by [Stuart Miller](https://github.com/sjmiller8182),
 [Paul Adams](https://github.com/PaulAdams4361),
 [Justin Howard](https://github.com/juhoward),
 and [Mel Schwan](https://github.com/schwan1).

# Project Summary

This project is an analysis of [lending data from Home Credit](https://www.kaggle.com/c/home-credit-default-risk/overview).
Predicting defaulting loans was the primary goal of the project.
Secondarily, we also segmented customers and provided an interpretation of the clusters.

## Analysis

This project is broken into three main sections

1. Data Exploration and Understanding
2. Predictive Modeling
3. Clustering Analysis and Segmentation

### Data Exploration

Explored the data to understand the type of features and the relationships between features.

* Provided detailed description of the features and assessed data types.
* Assessed missing values and removed features with too much data corruption.
* Reported interesting features from univariate and multivariate analyses.
* Engineered new features from understanding of given features.

[Report: MiniLab1 CRISP-DM](./notebooks/MiniLab1_CRISP-DM.ipynb): Notebook for data exploration phase.

### Predictive Modeling

* Assessed importance of features with logistic regression and random forests.
* Created three models for predicting loan defaults.
* Tuned model hyperparameters to improve performance.
* Assessed model performances and selected the best model for the application.
* Created and tuned three models for a regression task (secondary objective).
* Provided a model deployment plan.

[Report: MiniLab2 CRISP-DM](./notebooks/MiniLab2_CRISP-DM.ipynb): Notebook for predictive model development.

### Clustering Analysis and Segmentation

* Evaluated several methods for dimensionality reduction.
* Reduced the 300+ dimensions to 2 dimensions with an autoencoder.
* Clustered on the reduced dimension set.
* Provided an interpretation of the cluster.

[Report: Lab3 CRISP-DM](./notebooks/Lab3_CRISP-DM.ipynb): Notebook for clustering and segmentation.


## Main Conclusions

### Predictive Modeling

* We were able to create a list of important features.
* We developed a logistic model with AUC of 0.76. This model did not show evidence of over fitting.

### Clustering and Segmentation

* We were able to create 8 well defined clusters, which can be easily interpreted.
* We were also able to develop a model to classify new customers into the clusters with reasonable AUC.


## Additional Information

* The project repository can be found at [https://github.com/sjmiller8182/home-credit-default-risk](https://github.com/sjmiller8182/home-credit-default-risk).
* This analysis was conducted in Python using Numpy, Pandas, Scikit-Learn, and Keras.

