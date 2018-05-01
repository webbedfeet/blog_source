---
title: Surprising result when exploring Rcpp gallery
author: Abhijit Dasgupta
date: '2017-07-21'
categories:
  - R
slug: surprising-result-when-exploring-rcpp-gallery
---

I'm starting to incorporate more Rcpp in my R work, and so decided to spend some time exploring the [Rcpp Gallery](http://gallery.rcpp.org). One [example](http://gallery.rcpp.org/articles/faster-data-frame-creation/) by John Merrill caught my eye. He provides a C++ solution to transforming an list of lists into a data frame, and shows impressive speed savings compared to as.data.frame.

This got me thinking about how I do this operation currently. I tend to rely on the `do.call` method. To mimic the example in the Rcpp example:

````
a <- replicate(250, 1:100, simplify=FALSE)
b <- do.call(cbind, a)
````

For fairness, I should get a data frame rather than a matrix, so for my comparisons, I do convert `b` into a data frame. I follow the original coding in the example, adding my method above into the mix. Comparing times:

````
res <- benchmark(as.data.frame(a),
                 CheapDataFrameBuilder(a),
                 as.data.frame(do.call(cbind, a)),
                 order="relative", replications=500)
res[,1:4]
````

The results were quite interesting to me :)

                                  test replications elapsed relative
    3 as.data.frame(do.call(cbind, a))          500    0.36    1.000
    2         CheapDataFrameBuilder(a)          500    0.52    1.444
    1                 as.data.frame(a)          500    7.28   20.222

I think part of what's happening here is that as.data.frame.list expends overhead checking for different aspects of making a legit data frame, including naming conventions. The comparison to `CheapDataFrameBuilder` should really be with my barebones strategy. Having said that, the example does provide great value in showing what can be done using Rcpp.
