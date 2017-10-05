---
layout: post
title:  "Thompson Sampling"
categories: Statistics
tags: [Multi-armed Bandit]
excerpt: ""
---

#### Bayesian Learning
- For example, estimate the parameter $$p$$ ($$0<p<1, p \in \mathbb{R}$$) for a [Bernoulli distribution](https://en.wikipedia.org/wiki/Bernoulli_distribution). We are given ten sample as $$1, 1, 0, 0, 1, 0, 0, 1, 1, 0$$.
  - Frequentist: an esimation for $$p$$ with some confident
  - Bayes: a probability distribution about $$p$$, 
\$$ \begin{align}Pr[p=x|D] = \frac{Pr[D|p=x] \cdot Pr[p=x]}{Pr[D]} \propto Pr[D|p=x] \cdot Pr[p=x] \end{align} $$
    - prior: $$Pr[p=x]$$
    - posterior: $$Pr[p=x \vert D]$$
    - likelihodd: $$Pr[D \vert p=x]$$

#### [Thompson sampling](https://en.wikipedia.org/wiki/Thompson_sampling)
- Source Paper: Thompson William, On the likelihood that one unknown probability exceeds another in view of the evidence of two samples, 1933
- An old heuristics for multi-armed bandit (MAB) problem
- Bayesian philosophy
- The algorithm:
  - Assumption: each arm reward is generated from a probabbility distribution $$p_i$$
  - Steps (iterative):
    - For each arm, start with a prior on the probability distribution $$p_i$$
    - Observe data and upddate to a posterior $$p_i$$
    - Play each arm with its posterior probability of being the best arm
      - If $$X_i$$ is a random variable distributed as $$p_i$$, then it plays arm $$i$$ with probability $$Pr(X_i>\max_{j \neq i}X_j)$$. A quick implementation ([Monte Carlo generation](https://en.wikipedia.org/wiki/Monte_Carlo_method#Applied_statistics)) is to generate a sample from $$p_i$$ for each i and pull the arm whose sample is greatest.
    
#### Random Probability Matching
- Steven Scott, A modern Bayesian look at the multi-armed bandit, 2010
- Derived from Thompson sampling/Bayesian posterior sampling
- Discrete problems to continuous problems

