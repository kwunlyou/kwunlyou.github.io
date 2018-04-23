---
layout: post
title:  "Logistic Regression"
categories: Statistics
tags: [Machine Learning]
excerpt: ""
---
### 1. Introduction
#### 1.2 Statistical View
if $$K=2$$ and denote $$\beta=[\beta_{1,0}, \beta_{1}^T]$$, we can treat the dependent variable $$G$$ ($$G=1$$ or $$G=2$$) as an outcome of [Bernoulli trial](https://en.wikipedia.org/wiki/Bernoulli_trial), i.e.,

$$
g_i | x_i, \beta \sim Bernoulli(\sigma(\beta, x_i))
$$ 

where $$\sigma(\beta, x_i) = \frac{1}{1+\exp(\beta_{1,0}+\beta_{1}^T \cdot x_i)}$$ is the [**logistic function**](https://en.wikipedia.org/wiki/Logistic_function). And it is easy to verify

$$\log \frac{Pr(G=1|X=x)}{Pr(G=2|X=x)} = \beta_{1,0} + \beta_{1}^T \cdot x$$

#### 1.1 Motivation
[**Logistic regression**](https://en.wikipedia.org/wiki/Logistic_regression) arises from the desire to model the posterier probabilities of the $$K$$ classess by using a linear function in $$x$$, while the probabilities sum to $$1$$ and remain in $$[0, 1]$$, i.e.,

$$
\begin{align}
\log\frac{Pr(G=1|X=x)}{Pr(G=K|X=x)} &= \beta_{10} + \beta_{1}^T \cdot x \\
\vdots \\
\log\frac{Pr(G=K-1|X=x)}{Pr(G=K|X=x)} &= \beta_{K-1,0} + \beta_{K-1}^T \cdot x \\
\sum_{k=1}^{k=K}Pr(G=k|X=x) &= 1, \text{and} ~~0 \leq Pr(G=k|X=x) \leq 1
\end{align}
$$

The model has $$K-1$$ [**logit**](https://en.wikipedia.org/wiki/Logit) or [**log-odd**](https://en.wikipedia.org/wiki/Logit) transformations. The model can be rewrote as 

$$
\begin{align}
Pr(G=k|X=x) &= \frac{\exp(\beta_{k,0} + \beta_{k}^T \cdot x)}{1+\sum_{k=1}^{k=K-1}\exp(\beta_{k,0} + \beta_{k}^T \cdot x)}, ~~k \in \{1, \dots, K-1\} \\
Pr(G=K|X=x) &= \frac{1}{1+\sum_{k=1}^{K-1}\exp(\beta_{k,0} + \beta_{k}^T \cdot x)}
\end{align}
$$

Apparently, 

$$
\sum_{k=1}^{k=K}Pr(G=k|X=x) = 1, \text{and} ~~0 \leq Pr(G=k|X=x) \leq 1
$$

are satisfied implicitly. Although the last class is chosen as the deminator term, actually the results is independent with the choice and thus the choice can be arbitrary.

If denote the entire parameter set $$\{\beta_{1,0}, \beta_{1}^T, \dots, \beta_{K-1, 0}, \beta_{K-1}^T\}$$ as $$\theta$$, we have

$$
Pr(G=k|X=x) = p_k(x;\theta)
$$

### 2. Model Fitting
The logistic regression model is fitted by using [**maximum likehood estimation**](https://en.wikipedia.org/wiki/Maximum_likelihood_estimation). $$Pr(G|X)$$ can be considered as a conditional probability for some distribution such as [**multinomial distribution**](https://en.wikipedia.org/wiki/Multinomial_distribution).
The [**log-likelihood**](https://en.wikipedia.org/wiki/Likelihood_function#Log-likelihood) for $$N$$ observations is

$$
\mathscr{l}(\theta) = \sum_{i=1}^{N}\log p_{g_i}(x_i; \theta)
$$

where 

$$p_{g_i}(x_i; \theta) = Pr(G=g_i|X=x_i, \theta)$$

#### 2.1 Maximum Likelihood Estimation
To simplify the problem, we assume $$K=2$$. For convenience, we introduce another variable $$y$$ where $$y=1$$ if $$G=1$$ and $$y=0$$ if $$G=2$$.
Let $$p(x; \theta) = p_{1}(x; \theta)$$, and thus $$1 - p(x; \theta) = p_{2}(x; \theta)$$. 

The log-likelihood is 

$$
\begin{align}
\mathscr{l}(\beta) &= \sum_{i=1}^{N} \{ y_i\log p(x_i; \beta) + (1-y_i)\log (1-p(x_i; \beta)) \} \\
                   &= \sum_{i=1}^{N} \{ y_i(\beta^T \cdot  x) - \log(1 + exp(\beta^T \cdot x)) \}
\end{align}
$$

where $$\beta=[\beta_{1,0}, \beta_{1}^T]$$ and we assume **the vector $$x$$ contains a constant term $$1$$** for the intercept of the linear function.

To maximize the log-likelihood, we set the derivative w.r.t. $$\beta$$ equal to zero, and obtain **score euqations**, i.e.,

$$
0 = \frac{\partial \mathscr{l}(\beta)}{\partial \beta} = \sum_{i=1}^N (y_i - p(x_i; \beta)) x_i
$$

which has $$d+1$$ nonlinear equations. $$d$$ is the dimensionality of original $$x_i$$ without adding constant term $$1$$.
Since the first term of the current $$x_i$$ is $$1$$, we have 

$$
\sum_{i=1}^{N} y_i = \sum_{i=1}^N p(x_i; \beta)
$$

This makes sense because the expected number of $$1$$s should match the observed number.

To solve the nonlinear score equations, we can apply [**Newton's method**](https://en.wikipedia.org/wiki/Newton%27s_method). 
Therefore, the [**Hessian matrix**](https://en.wikipedia.org/wiki/Hessian_matrix) of the log-likehood is required, i,e,

$$
\frac{\partial ^2 \mathscr{l}(\beta)}{\partial \beta \partial \beta ^T} = - \sum_{i=1}^N x_i x_i^T p(x_i; \beta)(1-p(x_i; \beta))
$$

In each interation, the solution $$\beta$$ is updated as 

$$
\beta_{new} = \beta_{old} - (\frac{\partial ^2 \mathscr{l}(\beta)}{\partial \beta \partial \beta ^T})^{-1}_{\beta=\beta_{old}} (\frac{\partial \mathscr{l}(\beta)}{\partial \beta})_{\beta=\beta_{old}}
$$

##### 2.1.1 Iteratively Reweighted Least Squares
It is convenient to write the score equations and the Hessian matrix in matrix notations. Let $$\mathbf{y}$$ be the column vector with $$i$$th value as $$y_i$$,
$$\mathbf{X}$$ be a $$N \times (d+1)$$ matrix with $$i$$th row as $$x_i^T$$, and $$\mathbf{p}$$ is the column vector with $$i$$th value as $$p(x_i; \beta)$$

$$
\begin{align}
\frac{\partial \mathscr{l}(\beta)}{\partial \beta} &= \mathbf{X}^T(\mathbf{y} - \mathbf{p}) \\
\frac{\partial ^2 \mathscr{l}(\beta)}{\partial \beta \partial \beta ^T} &= - \mathbf{X}^T \mathbf{W} \mathbf{X} \\
\end{align}
$$

where $$\mathbf{W}$$ is a diagonal matrix with $$i$$th value $$p(x_i; \beta)(1-p(x_i; \beta)$$.

The solution update is this

$$
\begin{align}
\beta_{new} &= \beta_{old} + (\mathbf{X}^T\mathbf{W}\mathbf{X})^{-1} \mathbf{X}^T (\mathbf{y} - \mathbf{p}) \\
            &= (\mathbf{X}^T\mathbf{W}\mathbf{X})^{-1} \mathbf{X}^T \mathbf{W} (\mathbf{X} \beta_{old} + \mathbf{W}^{-1} (\mathbf{y}-\mathbf{p})) \\
            &= (\mathbf{X}^T \mathbf{W} \mathbf{X})^{-1} \mathbf{X}^T \mathbf{W} \mathbf{z}
\end{align}
$$

where $$\mathbf{z} = \mathbf{X} \beta_{old} + \mathbf{W}^{-1} (\mathbf{y}-\mathbf{p})$$ known as the **adjusted response**.

Therefore, in each iteration, $$\mathbf{p}$$ changes and thus as well as $$\mathbf{W}$$ and $$\mathbf{z}$$. 
The algorithm is so-called [**iteratively reweighted least squares**](https://en.wikipedia.org/wiki/Iteratively_reweighted_least_squares) (IRLS), 
because in the each iteration equivalently a weighted least square problem is solved:

$$
\beta_{new} \leftarrow \arg\min_{\beta} (\mathbf{z} - \mathbf{X}\beta)^T \mathbf{W} (\mathbf{z} - \mathbf{X}\beta)
$$

##### 2.1.2 Discussions
**Unique maximum**
- the cost function (log-likelihood) is concave function of $$\beta$$
- the Hessian matrix is negative definite as 
- therefore, there is a unique maximum

**Convergences:**
- $$\beta=0$$ is a good starting value but no convergence not guaranteed
- since the log-likelihood is concave, in general, the algorithm converges but overshooting can happen
- in the case the log-likelihood decreases, step size havling can guarantee convergence

**Overfitting:**
- severe overfitting when the data can be linearly seperated
- a solution is to introduce regularization

**Interpretation of the weighting matrix $$\mathbf{W}$$:**
- the diagnonal matrix $$\mathbf{W}$$ can be viewed as variances because

$$
\begin{align}
\mathbb{E}[y|x] & = p(x; \beta) \\
Var[y|x] & = \mathbb{E}(y^2|x) -  \mathbb{E}[t]^2 = p(x; \beta) -  p(x; \beta)^2 = p(x; \beta)(1 - p(x; \beta)) \\
\text{where the property}~~ y^2 = y ~~\text{is used}
\end{align}
$$

**Intepretaton of $$\mathbf{Z}$$:**
- Let $$z_i$$ be the $$i$$th value in $$\mathbf{z}$$, then we have

$$
\begin{align}
z_i &= \beta_{old}^T x_i + \frac{y_i - p(x_i; \beta_{old}))}{(1-p(x_i; \beta_{old})) p(x_i; \beta_{old})} \\
    &= \beta_{old}^T x_i + \frac{1}{(1-p(x_i; \beta_{old})) p(x_i; \beta_{old})} (y_i - p(x_i; \beta_{old}))) \\
    &= \beta_{old}^T x_i + \frac{\partial  \beta^T x_i}{\partial p(x_i; \beta)}|_{\beta = \beta_{old}} (y_i - p(x_i; \beta_{old})) \\
    &\approx \beta^T x_i \\
    &\text{where}~~\frac{\partial  \beta^T x_i}{\partial p(x_i; \beta)}|_{\beta = \beta_{old}} =  \frac{1}{(1-p(x_i; \beta_{old})) p(x_i; \beta_{old})} \\
    &\text{can be obtained from}~~ \beta^T x_i = \log \frac{p(x_i; \beta)}{1-p(x_i; \beta)}
\end{align}
$$
- This indicates the $$i$$th value of the IRLS solution is local linear approximation of the logistic function $$\beta^T x_i$$

#### 2.1.3 Multiclasse case
In the case of $$K \geq 3$$, the iteration of Newton's method can be also solved by using iterative reweighted least squares, 
but with a vector of $$K-1$$ responses and a nondiagonal weight matrix per observation.

### 3. Implementations
- [Apache Spark](https://spark.apache.org/docs/latest/ml-classification-regression.html#binomial-logistic-regression)
- [Implementing Sparse Logistic Regression in Spark](https://databricks.com/session/lessons-learned-implementing-sparse-logistic-regression-algorithm-apache-spark)
- [A local version of logistic regression implementation adapted from Apache Spark](http://eugenezhulenev.com/blog/2015/09/16/spark-ml-for-big-and-small-data/)
- [scikit-learn](http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html#sklearn.linear_model.LogisticRegression)
- [glmnet: Lasso and Elastic-Net Regularized Generalized Linear Models](https://cran.r-project.org/web/packages/glmnet/index.html)

### References:
- Friedman J, Hastie T, Tibshirani R. [The elements of statistical learning](https://web.stanford.edu/~hastie/ElemStatLearn/). New York: Springer series in statistics; 2009.
- Bishop CM. Pattern recognition and machine learning. springer; 2006.
