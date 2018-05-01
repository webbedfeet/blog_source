---
title: Quick and dirty parallel processing in R
author: Abhijit Dasgupta
date: '2010-04-30'
categories:
  - Computation
tags:
  - parallel computing
  - R
slug: quick-and-dirty-parallel-processing-in-r
---

R has some powerful tools for parallel processing, which I discovered while searching for ways to fully utilize my 8-core computer at work. What surprised me is how easy it is...about 6 lines of code, if that. Given that I wasn't allowed to install heavy duty parallel-processing systems like MPICH on the computer, I found that the library **SNOW** fit the bill nicely through its use of sockets. I also discovered the libraries **foreach** and **iterators**, which were released to the community by the development team at Revolution R. Using these 3 libraries, I could easily parallelize a transformation of my dataset where the transformations happened within each unique ID. The following code did the trick:

` library(foreach)
 library(doSNOW)
 cl <- makeCluster(6, type="SOCK") # using 6 nodes
 registerDoSNOW(cl)
 uID <- unique(ID)
 foreach(i=icount(length(uID)) %dopar% {
 transformData(dat[dat$ID==uID[i],])
 }
 stopCluster(cl)
 `
 Note that this is for a _multiprocessor single computer_. Doing this on a cluster may be more complicated, but this serves my purposes quite nicely. There are other choices for this, including the **multicore** library and others described in the [CRAN Task View](//cran.fhcrc.org/web/views/HighPerformanceComputing.html)

**Update**: I found that this strategy did not work for R 2.11 Windows versions, since `snow` is not properly spawning processes. However, there is a library ` doSMP` provided by _Revolution Analytics_ which gets around this problem. So replacing `doSNOW` with `doSMP` should do the trick.** **

**Update (7/25/2011):** It appears that SNOW does work again in R 2.13.0, the current version, on Windows. I've been using the snowfall package recently on my multi-core WinXP64 computer, and it works beautifully. **
**
