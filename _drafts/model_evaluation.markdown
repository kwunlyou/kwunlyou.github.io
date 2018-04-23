---
layout: post
title:  "Model Evaluation and Model Selection"
categories: Machine Learning
tags: []
excerpt: ""
---
- [pdf version](https://sebastianraschka.com/pdf/manuscripts/model-eval.pdf)

- [original post](https://sebastianraschka.com/blog/2016/model-evaluation-selection-part1.html)

- [normal approximation interval](https://en.wikipedia.org/wiki/Binomial_proportion_confidence_interval#Normal_approximation_interval) (to compute a confidence interval for a binomial proportion)
- Central limit theorem 
- Monte Carlo cross-validation (repeated hold out cross validation)
- Leave-One-Out Bootstrap
- Standard error of the mean
- [Student's t-distribution](https://en.wikipedia.org/wiki/Student's_t-distribution) and t-table
- [General linear model](https://en.wikipedia.org/wiki/Generalized_linear_model)
- [areaUnderROC  v.s. areaUnderPR](https://stats.stackexchange.com/questions/90779/area-under-the-roc-curve-or-area-under-the-pr-curve-for-imbalanced-data)
- [nested cross-validation](http://scikit-learn.org/stable/auto_examples/model_selection/plot_nested_cross_validation_iris.html)
- Cross validation
    - "the main idea behind cross-validation is that each sample in our dataset has the opportunity of being tested"
    - "The idea behind this approach is to reduce the pessimistic bias by using more training data in contrast to setting aside a relatively large portion of the dataset as test data."
    - "For instance, if we repeated a 5-fold cross-validation run 100 times, we would compute the performance estimate for 500 test folds report the cross-validation performance as the arithmetic mean of these 500 folds." "Furthermore, other researchers found that repeating k-fold cross-validation can increase the precision of the estimates while still maintaining a small bias"
    - "decreasing the value of k in k-fold cross-validation to small values (e.g., 2 or 3) also increases the variance on small datasets due to random sampling effects."
- Hyperparameter tuning 
    - grid search
    - random search
    - bayesian optimization
- One standard error rule
