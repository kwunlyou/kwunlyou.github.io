---
layout: post
title:  "Using Native BLAS/LAPACK in Apache Spark"
categories: [Library]
tags: [Apache Spark, BLAS, LAPACK, netlib-java]
excerpt: ""
---
- [Apache Spark](https://github.com/apache/spark/blob/branch-2.2/docs/ml-guide.md): "Due to licensing issues with runtime proprietary binaries, we do not include netlib-java's native proxies by default." 
- A [question](https://stackoverflow.com/questions/37848216/how-to-configure-high-performance-blas-lapack-for-breeze-on-amazon-emr-ec2) in StackOverflow
- [sbt-assembly](https://github.com/sbt/sbt-assembly#using-published-plugin)

**Solution**
- change `spark.driver.extraClassPath` in `spark-default.conf`; do not use `--driver-class-path` and it will overwrite other paths
- default blas/lapack in EMR is very slow; compile/install OpenBlas instead

**Questions**
- Need more test for LR problem
- Other elegant solution?
  - [Discussion](https://stackoverflow.com/questions/29099115/spark-submit-add-multiple-jars-in-classpath) in StackOverflow
- How about pyspark?
  - using 'packages'??

