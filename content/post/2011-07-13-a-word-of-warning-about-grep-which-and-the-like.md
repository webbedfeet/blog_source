---
title: A word of warning about grep, which and the like
author: Abhijit Dasgupta
date: '2011-07-13'
tags:
  - R
slug: a-word-of-warning-about-grep-which-and-the-like
---

I've often selected columns or rows of a data frame using `grep` or `which`, based on some property. That is inherently sound, but the trouble comes when you wish to remove rows or columns based on that `grep` or `which` call, e.g.,
````r
dat <- dat[,-grep('\\.1', names(dat))]
````
which would remove columns with a .1 in the name. This is fine the first time around, but if you forget and re-run the code, `grep('\\.1',names(dat))` gives a vector of length 0, and hence `dat` becomes a data.frame with 0 columns. The function `which` also has similar pitfalls, as demonstrated in a recent R-help posting by David Winsemius. I find a more reliable method is to do
````r
dat <- dat[,setdiff(1:ncol(dat),grep('\\.1',names(dat)))]
````
which will always give the right number of columns. Other suggestions for getting around this issue are welcomed in the comments.
