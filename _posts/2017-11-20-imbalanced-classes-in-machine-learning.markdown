---
layout: post
title:  "Imbalanced Classes in Machine Learning"
categories: [Machine Learning]
tags: [Imbalanced Classes]
excerpt: "This short post aims to very briefly cover related topics on imbalanced classes in machine learning. In machine learning, imbalanced classes is very common in practice. However, no algorithm can deal with the issue directly and other auxiliary methods must be introduced explicitly to resolve the challenge"
---

### 1. Introduction
This short post aims to very briefly cover related topics on imbalanced classes in machine learning. In machine learning, imbalanced classes is very common in practice. However, no algorithm can deal with the issue directly and other auxiliary methods must be introduced explicitly to resolve the challenge. 

### 2. Methods
In general, there are following types of methods:
- do nothing (if lucky enough)
- data-level:
  - decrease majority size:
    - using clustering methods such as K-means
    - [Condensed Nearest Neighbours](https://doi.org/10.1109/TSMC.1976.4309452)
      - [Tomek's links](http://contrib.scikit-learn.org/imbalanced-learn/stable/under_sampling.html#tomek-links)
  - increase minority size:
    - synthesize new samples: 
      - [SMOTE](https://www.cs.cmu.edu/afs/cs/project/jair/pub/volume16/chawla02a-html/chawla2002.html) (Synthetic Minority Over-sampling Technique)
      - [ADASYN](http://sci2s.ugr.es/keel/pdf/algorithm/congreso/2008-He-ieee.pdf) (Adaptive Synthetic Sampling Approach for Imbalanced Learning)
- algorithm-level:
  - class weights
  - decision thresholds 
  - ensemble techniques 
    - boostrap aggregating (bagging) [[code]](https://github.com/silicon-valley-data-science/learning-from-imbalanced-classes/blob/master/blagging.py)
      - Pros: Good for reducing variance, robust to noise, void overfitting
      - Cons: Can be worse if single estimator is bad (use boosting)
    - boosting (weak learners to a strong learner)
      - Adaptive Boosting (AdaBoost)
      - Gradient Boosting
  - remove minority examples and switch to an anomaly detection problem
    - [Isolation-Based Anomaly Detection](https://cs.nju.edu.cn/zhouzh/zhouzh.files/publication/tkdd11.pdf)
    - [Efficient Anomaly Detection by Isolation Using Nearest Neighbour Ensemble](http://ieeexplore.ieee.org/document/7022664/)
- others:
  - [All the Data and Still Not Enough](https://www.oreilly.com/learning/all-the-data-and-still-not-enough)
  - [Transfer learning](http://dl.acm.org/citation.cfm?id=2597426){:target="blank"}
  
### 3. Evaluation metrics
- Do not use `accuracy`, `precision`, `recall`, `F1 score` (hard cutoff on predicted probabilties); use `ROC` curve, `PR` curve, `AUC`
- `PR` curve is prefered than `ROC` curve
  - [2015 - The Precision-Recall Plot Is More Informative than the ROC Plot When Evaluating Binary Classifiers on Imbalanced Datasets](http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0118432)
  - [2006 - The Relationship Between Precision-Recall and ROC Curves](http://pages.cs.wisc.edu/~jdavis/davisgoadrichcamera2.pdf)
  - [2005 - An introduction to ROC analysis](http://people.inf.elte.hu/kiss/11dwhdm/roc.pdf)

### 4. Resources:
- Podcast: 
  - [Data Sciece at Home: How to handle imbalanced datasets](https://datascienceathome.podbean.com/e/imbalanced-datasets/){:target="blank"}
- Blog: 
  - [Learning from Imbalanced Classes](https://www.svds.com/learning-imbalanced-classes/){:target="blank"}
  - [Imbalanced Classes FAQ](https://www.svds.com/imbalanced-classes-faq/){:target="blank"}
  - Handling Class Imbalance with R and Caret - Caveats when using the AUC [[Part 1]](http://dpmartin42.github.io/blogposts/r/imbalanced-classes-part-1){:target="blank"} [[Part2]](http://dpmartin42.github.io/blogposts/r/imbalanced-classes-part-2){:target="blank"}
- Paper: 
  - [A Survey of Predictive Modelling under Imbalanced Distributions](https://arxiv.org/abs/1505.01658){:target="blank"}
  - [Class Imbalance, Redux](https://pdfs.semanticscholar.org/a8ef/5a810099178b70d1490a4e6fc4426b642cde.pdf)
  - [Using Random Forest to Learn Imbalanced Data](http://statistics.berkeley.edu/sites/default/files/tech-reports/666.pdf)
- Code:
  - sklearn
    - [class_weight](http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html){:target="blank"} in logistic regression
    - [sklearn.model_selection.StratifiedKFold](http://scikit-learn.org/stable/modules/generated/sklearn.model_selection.StratifiedKFold.html)
    - [sklearn.calibration.CalibratedClassifierCV](http://scikit-learn.org/stable/modules/generated/sklearn.calibration.CalibratedClassifierCV.html)
  - [imbalanced-learn](https://github.com/scikit-learn-contrib/imbalanced-learn)
  - The R package [unbalanced](https://cran.r-project.org/web/packages/unbalanced/index.html)

