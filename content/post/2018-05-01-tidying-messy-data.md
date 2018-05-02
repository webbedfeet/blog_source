---
title: Tidying messy Excel data (Introduction)
author: Abhijit Dasgupta
date: '2018-05-01'
slug: tidying-messy-data-1
categories: ["R"]
tags:
  - R
  - messy data
header:
  caption: ''
  image: ''
---

### Personal expressiveness, or how data is stored in a spreadsheet

When you get data from a broad research community, the variability in how that data is 
formatted and stored is truly astonishing. Of course there are the standardized formats that are output from machines, like Next Generation Sequencing and other automated systems. That is a saving grace!

But for smaller data, or data collected in the lab, the possibilities are truly endless! You can get every possiblle color-coding of rows, columns and cells, merged cells, 
hidden columns and rows, and inopportune blank spaces that convert numbers to characters, 
and notes where there should be numbers. That's just from the more organized spreadsheets. Then you get multiple tables in the same spreadsheet, ad hoc computations in some cells, 
cells copied by hand (with error), and sundry other variations on this theme. In other words, it can be a nightmare scenario for the analyst. To wit, 

<!--html_preserve-->{{% tweet "989274016807448577" %}}<!--/html_preserve-->

In thinking about this post, I went back and looked at the documentation of the [`readxl`](http://readxl.tidyverse.org) package, which has made reading Excel files into R a lot easier than before. This package is quite powerful, so as long as data are in a 
relatively clean tabular form, this tool can pull it into R; see [these](http://readxl.tidyverse.org/articles/sheet-geometry.html) [vignettes](http://readxl.tidyverse.org/articles/sheet-geometry.html) to get a real sense of how to process decently behaved Excel files with R. 

> On a side note, how many ways of importing Excel files into R can you name, or have you used?

The `readxl` package has been my friend for a while, but then I received some well-intentioned spreadsheets that even `readxl` wouldn't touch, despite much coaxing. Now, I had two options: ask the collaborator to re-format the spreadsheet (manually, of course :smile:), which in my experience is a disaster waiting to happen; or just take the spreadsheet as is and figure out how to import it into R. I almost always take the latter route, and `tidyxl` is my bosom friend in this effort. In the next part of this series, I'll describe my experiences with `tidyxl` and why, every time I use it, I say a small blessing for the team that created it. 

### Resources

1. [tidyeval meets Spreadsheet Hell](http://rpubs.com/jennybc/untangle-tidyeval)
2. [Carpentries lesson on spreadsheet practices](http://www.datacarpentry.org/spreadsheet-ecology-lesson/)
3. [`readxl` workflows](http://readxl.tidyverse.org/articles/articles/readxl-workflows.html)
