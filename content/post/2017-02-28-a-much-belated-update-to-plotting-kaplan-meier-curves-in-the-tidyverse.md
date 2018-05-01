---
title: A (much belated) update to plotting Kaplan-Meier curves in the tidyverse
author: Abhijit Dasgupta
date: '2017-02-28'
categories:
  - R
slug: a-much-belated-update-to-plotting-kaplan-meier-curves-in-the-tidyverse
---

One of the [most popular posts](https://statbandit.wordpress.com/2014/04/01/kaplan-meier-plots-using-ggplots2-updated/) on this blog has been my attempt to create Kaplan-Meier plots with an aligned table of persons-at-risk below it under the ggplot paradigm. That post was last updated 3 years ago. In the interim, Chris Dardis has built upon these attempts to create a much more stable and feature-rich version of this function in his package [survMisc](https://cran.r-project.org/web/packages/survMisc/); the function is called [autoplot](https://www.rdocumentation.org/packages/survMisc/versions/0.5.4/topics/autoplotTableAndPlot).
