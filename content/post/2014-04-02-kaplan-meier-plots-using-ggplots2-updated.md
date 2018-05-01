---
title: Kaplan-Meier plots using ggplots2 (updated)
author: Abhijit Dasgupta
date: '2014-04-01'
tags:
  - ggplot2
  - Kaplan-Meier
  - R
  - survival analysis
slug: kaplan-meier-plots-using-ggplots2-updated
---

About 3 years ago I published some code on this blog to draw a Kaplan-Meier plot using ggplot2. Since then, ggplot2 has been updated (from 0.8.9 to 0.9.3.1) and has changed syntactically. Since that post, I have also become comfortable with Git and Github. I have updated the code, edited it for a small error, and published it in a Gist. This gist has two functions, ggkm (basic Kaplan-Meier plot) and ggkmTable (enhanced Kaplan-Meier plot with table showing numbers at risk at various times).

This gist is published [here](https://gist.github.com/webbedfeet/1faf78996b61264e5e9e\). If you find errors or want to enhance these functions, please fork, update and send me a link to your fork in the comments. I'll pull and merge them. Unfortunately Github doesn't allow pull requests directly for gists (see [here](http://stackoverflow.com/questions/8758612/can-i-make-a-pull-request-on-a-gist-on-github) for the StackOverflow answer I'm basing this on).

If you want to go back to the original post, you can read it [here](http://statbandit.wordpress.com/2011/03/08/an-enhanced-kaplan-meier-plot/).
