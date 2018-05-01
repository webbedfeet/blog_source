---
title: Reading fixed width formats in the Hadleyverse
author: Abhijit Dasgupta
date: '2015-04-19'
categories:
  - Data Science
  - R
slug: reading-fixed-width-formats-in-the-hadleyverse
---

This is an update to a [previous post](https://statbandit.wordpress.com/2014/11/10/laf-ing-about-fixed-width-formats/) on reading fixed width formats in R.

A new addition to the Hadleyverse is the package [readr](https://github.com/hadley/readr), which includes a function `read_fwf` to read fixed width format files. I'll compare the LaF approach to the readr approach using the same dataset as before. The variable `wt` is generated from parsing the Stata load file as before.

I want to read all the data in two columns: DRG and HOSPID. The LaF solution for this is:
````
library(LaF)
d <- laf_open_fwf('NIS_2008_Core.ASC', column_widths=wt$widths,
                  column_types=wt$classes, column_names = wt$varnames)
d1 <- d[,c('DRG','HOSPID')]
````

For readr, we use the following code:
````
library(readr)
x <- as.data.frame(fwf_widths(wt$widths, wt$varnames))
fwfpos <- fwf_positions(x[c(15,62),1], x[c(15,62),2], col_names=c(x[c(15,62),3]))
d1 <- read_fwf('NIS_2008_Core.ASC', fwfpos)
````

As a comparison of times on my MacBook Air (2014) with 8GB memory and a 1.4 GHz Intel Core i5 and a Solid State Disk, the LaF approach took 20.73s (system time 2.52s), while the readr solution takes 40.23s (system time 10.55s). While readr has improved things a lot compared to `read.fwf` and other base R solutions, the LaF approach is still more efficient.
