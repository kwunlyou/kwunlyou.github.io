---
layout: post
title:  "PCA and SVD on Large Matrices"
categories: [Statistics, Mathematics]
tags: [Linear Algebra, Matrix Decomposition]
excerpt: ""
---
#### Mathematical Background
- $$A^* A = V \Sigma^* \Sigma V^*$$
1. Form $$A^* A$$
2. compute the eigenvalues decomposition $$A^* A = V \Lambda V^*$$
3. let $\Sigma$ be the $$m \times n$$ nonnegative diagonal square root of $$\Lambda$$
4. solve the system $$U \Sigma = A V$$ for unitary $$U$$ (e.g., via QR decomposition)

#### Others
- [Book] Numerical linear algebra
  - Not stable to compute the eigen values of $$A^tA$$, especially for insignificant eigen values

- Randomized PCA
  - Implementations
    - fbpca
      - [post](https://research.fb.com/fast-randomized-svd/)
      - [code](https://github.com/facebook/fbpca/blob/master/fbpca.py)
    - scikit-learn
      - [code](https://github.com/scikit-learn/scikit-learn/blob/master/sklearn/utils/extmath.py)
  - Benckmark:
    - [code](https://github.com/scikit-learn/scikit-learn/blob/master/benchmarks/bench_plot_randomized_svd.py)
    
- Incremental PCA
  - [docs](http://scikit-learn.org/stable/modules/generated/sklearn.decomposition.IncrementalPCA.html#sklearn.decomposition.IncrementalPCA)

- PCA and SVD in Apache Spark
  - [blog](https://databricks.com/blog/2014/07/21/distributing-the-singular-value-decomposition-with-spark.html) about PCA/SVD in Spark 
  - The first PCA/SVD implementation by Reza Zadeh
    - [code](https://github.com/apache/spark/pull/88)
    - PCA: samples centering --> call SVD
    - SVD: compute $$A^tA$$ --> call `jblas.Singular.SparseSVD`
  - Improved by Pu Li
    -[issue](https://issues.apache.org/jira/browse/SPARK-1782)

- A [blog post](http://amedee.me/post/pca-large-matrices/) about PCA on large matrices


