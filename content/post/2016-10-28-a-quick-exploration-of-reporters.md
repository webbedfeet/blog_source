---
title: A quick exploration of the ReporteRs package
author: Abhijit Dasgupta
date: '2016-10-28'
tags:
  - R
slug: a-quick-exploration-of-reporters
---

The package [`ReporteRs`](http://davidgohel.github.io/ReporteRs/) has been getting some play on the interwebs this week, though it's actually been around for a while. The nice thing about this package is that it allows writing Word and PowerPoint documents in an OS-independent fashion unlike some earlier packages. It also allows the editing of documents by using bookmarks within the documents.

This quick note is just to remind me that the structure of `ReporteRs` works beautifully with the piping conventions of `magrittr`. For example, a report I wrote today maintained my flow while writing R code to create the report.

````rlibrary(ReporteRs)
library(magrittr)

mydoc <- docx() %>%
  addParagraph(value = 'Correlation matrix', style='Titre2') %>%
  addParagraph(value='Estimates') %>%
  addFlexTable(FlexTable(cormat)) %>%
  addParagraph(value = 'P-values') %>%
  addFlexTable(FlexTable(corpval)) %>%
  addParagraph(value = "Boxplots", style='Titre2') %>%
  addPlot(fun=print, x = plt, height=3, width=5) %>%
  writeDoc(file = 'Report.docx)
````

Note that `plt` is a ggplot object and so we actually have to print it rather than just put the object in the `addPlot` command.

This was my first experience in a while using `ReporteRs`, and it seemed pretty good to me.
