---
layout: post
title:  "lr"
categories: Machine Learning
tags: [Classification]
excerpt: ""
---

#### Logistic Regression
- Statistical View / Generative Model 

$$y_i | \beta, x_i \sim Bernoulli(\sigma(\beta, x_i))$$, 

where 

$$\sigma(\beta, x) = \frac{1}{1+e^(-\beta \cdot x )}$$ is the **sigmoid** function.

$$\log(\frac{P[y=1]}{P[y=0]}) = \beta \cdot x$$

- Generalized Linear Model

- [Implementing Sparse Logistic Regression in Spark](https://databricks.com/session/lessons-learned-implementing-sparse-logistic-regression-algorithm-apache-spark)
