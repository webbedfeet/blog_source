---
title: Newer dplyr!!
author: Abhijit Dasgupta
date: '2014-05-21'
categories:
  - R
slug: newer-dplyr
---

Last week [Statistical Programming DC](http://datacommunitydc.org/blog/stats-prog-dc/) had a great meetup with my partner-in-crime Marck Vaisman talking about [data.table](http://datatable.r-forge.r-project.org/) and [dplyr](https://github.com/hadley/dplyr) as powerful, fast R tools for data manipulation in R. Today [Hadley Wickham ](http://had.co.nz/)[announced](http://blog.rstudio.org/2014/05/21/dplyr-0-2) the release of dplyr v.0.2, which is packed with new features and incorporates the "piping" syntax from [Stefan Holst Bache](http://www.stefanbache.dk/)'s [magrittr](https://github.com/smbache/magrittr) package. I suspect that these developments will change the semantics of working in R, specially during the data munging phase. I think the jury is still out on whether the "piping" model for function chaining will lead to better (and not just more jumbled) coding practice, but for some of my use cases, specially with the previous version of dplyr, it made me happier than before.
