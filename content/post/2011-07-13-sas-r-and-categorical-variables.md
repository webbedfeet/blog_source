---
title: SAS, R and categorical variables
author: Abhijit Dasgupta
date: '2011-07-13'
categories:
  - Computation
tags:
  - R
  - SAS
slug: sas-r-and-categorical-variables
---

One of the disappointing problems in SAS (as I need PROC MIXED for some analysis) is to recode categorical variables to have a particular reference category. In R, my usual tool, this is rather easy both to set and to modify using the  `relevel` command available in base R (in the stats package). My understanding is that this is actually easy in SAS for GLM, PHREG and some others, but not in PROC MIXED. (Once again I face my pet peeve about the inconsistencies within a leading commercial product and market "leader" like SAS). The easiest way to deal with this, I believe, is to actually create the dummy variables by hand using ifelse statements and use them in the model rather than the categorical variables themselves. If most of the covariates are not categorical, this isn't too burdensome.

I'm sure some SAS guru will comment on the elegant or "right" solution to this problem.
