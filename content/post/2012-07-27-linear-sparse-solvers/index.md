+++
title = "Linear Sparse Solvers"
subtitle = ""

# Add a summary to display on homepage (optional).
summary = "Recently I have to accelerate my last code and most of time are spent on solving linear sparse systems. Since my coefficient matrix is a symmetric positive definite matrix (s.p.d.), I always use CHOLMOD. But its performance cannot reach my requirement, I tried to search another better solver."

date = 2012-07-27T00:00:00+01:00
draft = false

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = []

# Is this a featured post? (true/false)
featured = false

# Tags and categories
# For example, use `tags = []` for no tags, or the form `tags = ["A Tag", "Another Tag"]` for one or more tags.
tags = ["CHOLMOD", "Linear Sparse Solver", "MATLAB", "MKL", "PCG"]
categories = ["Numerical Computing"]

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["deep-learning"]` references 
#   `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
# projects = ["internal-project"]

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder. 
[image]
  # Caption (optional)
  caption = ""

  # Focal point (optional)
  # Options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight
  focal_point = ""
+++

<p>Recently I have to accelerate my last code and most of time are spent on solving linear sparse systems. Since my coefficient matrix is a symmetric positive definite matrix (s.p.d.), I always use <a href="http://www.cise.ufl.edu/research/sparse/cholmod/">CHOLMOD </a>. But its performance cannot reach my requirement, I  tried to search another better solver. Then I tried <a href="http://www.mathworks.com/help/techdoc/ref/mldivide.html">backslash</a>(\) in MATLAB, the build-in <a href="http://en.wikipedia.org/wiki/Conjugate_gradient_method">preconditioned conjugate gradient</a> (PCG) method in <a href="http://eigen.tuxfamily.org/index.php?title=Main_Page">Eigen</a>, PARDISO in <a href="http://software.intel.com/en-us/articles/intel-mkl/">intel MKL</a> and PCG in intel MKL.</p>

<p>There is a comparison article (from <a href="http://www.cise.ufl.edu/~davis/" >Tim Davis</a>) of CHOLMOD with PCG method <a href="http://www.cise.ufl.edu/research/sparse/pcg_compare/" >[link]</a>. The main idea is that CHOLMOD performs bettter than PCG in most cases. However, unfortunately from my experiments it seems that PCG is more suitable than direct solvers (CHOLMOD and MKL) in my case. PCG need less time than direct solvers (CHOLMOD and MKL) to get similar results in my problem. Here is a figure roughly showing my results:</p>

<p class="text-center"><img title="time" src="/img/time.png" alt="" width=100% height=100%><strong>NOTE: x axis represents the size of the coefficient matrix and y axis represents the solving time.</strong></p>

<p>However, in my problem I have to solve lots of times linear sparse systems. It is not as fast as I want. Probably I have to improve the theory part to nail the problem now? Or is there another other better solvers? Je ne sais pas. :(</p>
