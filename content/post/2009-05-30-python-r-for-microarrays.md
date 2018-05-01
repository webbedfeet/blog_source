---
title: Python & R for microarrays
author: Abhijit Dasgupta
date: '2009-05-30'
slug: python-r-for-microarrays
---

Recently I have been processing Affymetrix Exon 1.0 arrays for a collaborator. Our aim is to associate gene expression with chemotherapy-related survival. For this , as an initial pass, I'm doing gene-by-gene Cox regression with genes as a single covariate. The problem using R directly is that the data file is HUGE (we're measuring over 1M genes here). I've adapted my usual R-based pipeline to use python instead. Python is much faster at reading large data sets into memory, as well as looping over the data set. I use Gautier's excellent rpy2 interfact between R and Python to do my Cox regression in R. Basically, as I loop over the genes I create a dictionary object which gets transformed to a data.frame in R, which is the namespace on which I run coxph. I then pipe back the p-value and hazard ratio to python. Finally I write the hazard ratio and p-value, as well as the probe ID, to a file (hard drive space seems cheaper operationally than memory for storage). I then use matplotlib to generate "volcano plots" for a first visualization, and also use python to summarize this derived data.

I could get through a lot of the analysis in this Python-rpy2-R pipeline in the same time it took me to load the data into R itself.
