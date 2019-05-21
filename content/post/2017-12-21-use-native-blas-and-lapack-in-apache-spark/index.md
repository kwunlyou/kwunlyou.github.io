+++
title = "Use Native BLAS/LAPACK in Apache Spark"
subtitle = ""

# Add a summary to display on homepage (optional).
summary = "The warning messages are often displayed when you use MLlib in Apache Spark. It means native BLAS implementations are not rightly installed or configured for your Apache Spark. A pure Java implementation is used which could harm the performance."

date = 2017-12-21T00:00:00+01:00
draft = false

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = []

# Is this a featured post? (true/false)
featured = false

# Tags and categories
# For example, use `tags = []` for no tags, or the form `tags = ["A Tag", "Another Tag"]` for one or more tags.
tags = ["Apache Spark", "BLAS", "LAPACK", "Breeze", "netlib-java"]
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

### 1. Problem statement

```
17/12/21 11:11:56 WARN BLAS: Failed to load implementation from: com.github.fommil.netlib.NativeSystemBLAS
17/12/21 11:11:56 WARN BLAS: Failed to load implementation from: com.github.fommil.netlib.NativeRefBLAS
```

The warning messages are often displayed when you use [MLlib](https://spark.apache.org/docs/latest/ml-guide.html) in [Apache Spark](https://spark.apache.org/).
It means native [BLAS](http://www.netlib.org/blas/) implementations are not rightly installed or configured for your Apache Spark. A pure Java implementation is used which could harm the performance. See [[1]](#ref_1) for more information.

The official Spark document [[2]](#ref_2) has an explanation about the warning message

>MLlib uses the linear algebra package Breeze, which depends on netlib-java for optimised numerical processing. If native libraries are not available at runtime, you will see a warning message and a pure JVM implementation will be used instead.
{: style="font-size: 100%"}

and

>Due to licensing issues with runtime proprietary binaries, we do not include netlib-java’s native proxies by default.
{: style="font-size: 100%"}

### 2. Solutions
As stated in the official Spark document [[2]](#ref_2),

>To configure netlib-java / Breeze to use system optimised binaries, include com.github.fommil.netlib:all:1.1.2 (or build Spark with -Pnetlib-lgpl) as a dependency of your project 
{: style="font-size: 100%"}

there are two kinds of solutions

1. rebuild Apache Spark
2. configure your project

The first one is almost impossible in some scenario such as Amazon EMR. This post focus on the second solution instead.

#### 2.1 Aside
Most of the linear algebra related functions in Spark MLlib are based on [Breeze](https://github.com/scalanlp/breeze) which is a numerical processing library for Scala, while some of them are directly based on the low level library [netlib-java](https://github.com/fommil/netlib-java) which is also used by [Breeze](https://github.com/scalanlp/breeze). In addition, Spark MLlib has some non-BLAS in-house implementations as well. 

In [netlib-java](https://github.com/fommil/netlib-java), the implementations of BLAS/LAPACK are provided by 

- "[F2J](http://icl.cs.utk.edu/f2j/) to ensure full portability on the JVM"
- "self-contained native builds using the reference Fortran from [netlib.org](http://www.netlib.org/)"
- "delegating builds that use machine optimised system libraries"

The relation is illustrated as the figure. In this post, we are trying to configure and use system-provided BLAS (in green).

<p style="text-align:center;"><img src="/img/spark_blas.png" alt="Spark BLAS" style="width: 60%;"/></p>

#### 2.2 Steps

##### Step 1:
Make sure a native BLAS/LAPACK implementation is installed such as [ATLAS](http://math-atlas.sourceforge.net/), [Intel MKL](https://software.intel.com/mkl), and [OpenBLAS](https://github.com/xianyi/OpenBLAS). [OpenBLAS](https://github.com/xianyi/OpenBLAS) generally has an excellent performance among free implementations. If you work on macOS, its [vecLib](https://developer.apple.com/documentation/accelerate/veclib) contains Apple's highly tuned implementation of BLAS/LAPACK. 

##### Step 2:
As sugguested in the official Spark documeny [[2]](#ref_2), include `com.github.fommil.netlib:all:1.1.2` in your project to use system optimized binaries. 

However, this is not enough. If you use [sbt](https://www.scala-sbt.org/), you have to use [sbt-assembly](https://github.com/sbt/sbt-assembly#using-published-plugin) to generate a fat `JAR` for your project in order to include netlib-java.

##### Step 3:
Add your generated fat `JAR` to `spark.driver.extraClassPath` and `spark.executor.extraClassPath` in `spark-default.conf`. Do not use `--driver-class-path` or `--jars` when you `spark-submit` your jobs, it does not work (I also want to know why).

For `pyspark` jobs, you only need to do the same configuration in order to use native BLAS.

NOTE: frequently changing `spark-default.conf` is not convenient. Instead, you can prepare two `JAR`s, one is for your project and one is for netlib-java.

### 3. Others

#### 3.1 Amazon Linux
As some people said [[3]](#ref_3), the BLAS/LAPACK installed in Amazon Linux does not perform well. We can install OpenBLAS instead. 
Here is a bash script to install OpenBLAS in Amazon Linux:

```bash
#!/bin/bash

set -e

sudo yum install -y git
git clone https://github.com/xianyi/OpenBlas.git
cd OpenBlas/
make clean
make -j
sudo mkdir /usr/lib64/OpenBLAS
sudo chmod o+w,g+w /usr/lib64/OpenBLAS/
make PREFIX=/usr/lib64/OpenBLAS install
sudo rm /etc/ld.so.conf.d/atlas-x86_64.conf
sudo ldconfig
sudo ln -sf /usr/lib64/OpenBLAS/lib/libopenblas.so /usr/lib64/libblas.so
sudo ln -sf /usr/lib64/OpenBLAS/lib/libopenblas.so /usr/lib64/libblas.so.3
sudo ln -sf /usr/lib64/OpenBLAS/lib/libopenblas.so /usr/lib64/libblas.so.3.5
sudo ln -sf /usr/lib64/OpenBLAS/lib/libopenblas.so /usr/lib64/libblas.so.3.5.0
sudo ln -sf /usr/lib64/OpenBLAS/lib/libopenblas.so /usr/lib64/liblapack.so
sudo ln -sf /usr/lib64/OpenBLAS/lib/libopenblas.so /usr/lib64/liblapack.so.3
sudo ln -sf /usr/lib64/OpenBLAS/lib/libopenblas.so /usr/lib64/liblapack.so.3.5
sudo ln -sf /usr/lib64/OpenBLAS/lib/libopenblas.so /usr/lib64/liblapack.so.3.5.0
```

#### 3.2 The multi-thread issue
As presented in the issue [Spark-21305](https://issues.apache.org/jira/browse/SPARK-21305), BLAS with multi-thread support can cause worse performance because it conflicts with Spark executors. Therefore, it is better to disable multi-thread.

#### 3.3 Paper
[[4]](#ref_4) is a bit out of date but is still very worth to read. It gives lots of details about implementations in Spark and experimental results using different BLAS implementations.

#### 3.4 Motivation
This post is originated from reading [[3]](#ref_3). I found lots of related posts but they are either not complete or out of date. Thus, I decide to record all what I read during solving the problem. All comments are welcome.

  
### References:
<a name="ref_1" style="color:black">1.</a> [Improving BLAS library performance for MLlib](http://www.spark.tc/blas-libraries-in-mllib/)

<a name="ref_2" style="color:black">2.</a> [MLlib Guide](https://spark.apache.org/docs/latest/ml-guide.html#dependencies)

<a name="ref_3" style="color:black">3.</a> [A question in Stackoverflow](https://stackoverflow.com/questions/37848216/how-to-configure-high-performance-blas-lapack-for-breeze-on-amazon-emr-ec2)

<a name="ref_4" style="color:black">4.</a> [Matrix Computations and Optimization in Apache Spark](https://arxiv.org/abs/1509.02256)
