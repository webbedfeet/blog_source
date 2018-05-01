---
title: Copying tables from R to Outlook
author: Abhijit Dasgupta
date: '2017-02-28'
categories:
  - R
slug: copying-tables-from-r-to-outlook
---

I work in an ecosystem that uses Outlook for e-mail. When I have to communicate results with collaborators one of the most frequent tasks I face is to take a tabular output in R (either a summary table or some sort of tabular output) and send it to collaborators in Outlook. One method is certainly to export the table to Excel  and then copy the table from there into Outlook. However, I think I prefer another method which works a bit quicker for me.

I've been writing full reports using [Rmarkdown](http://rmarkdown.rstudio.com) for a while now, and it's my preferred report-generation method. Usually I use `knitr::kable` to generate a Markdown version of a table in R. I can then copy the generated Markdown version of the table into a Markdown editor (I use [Minimalist Markdown Editor](https://chrome.google.com/webstore/detail/minimalist-markdown-edito/pghodfjepegmciihfhdipmimghiakcjf)), then just copy the HTML-rendered table from the preview pane to Outlook. This seem  to work pretty well for me
