---
title: The need for documenting functions
author: Abhijit Dasgupta
date: '2014-05-22'
categories:
  - Computation
tags:
  - R
  - RStudio
slug: the-need-for-documenting-functions
---

My current work usually requires me to work on a project until we can submit a research paper, and then move on to a new project. However, 3-6 months down the road, when the reviews for the paper return, it is quite common to have to do some new analyses or re-analyses of the data. At that time, I have to re-visit my code!

One of the common problems I (and I'm sure many of us) have is that we tend to hack code and functions with the end in mind, just getting the job done. However, when we have to re-visit code when it's no longer fresh in our memory, it takes significant time to decipher what some code snippet or function is doing, or why we did it in the first place. Then, when our paper gets published and a reader wants our code to try, it's a bear getting it into any kind of shareable form. Recently I've had both issues, and decided, enough was enough!!

R has a fantastic package [roxygen2](http://cran.r-project.org/web/packages/roxygen2/index.html) that makes documenting functions quite easy. The documentation sits just above the function code, so it is there front and center. Taking 2-5 minutes to write even a bare-bones documentation, that includes

  * what the function does

  * what are the inputs (in English) and their required R class

  * what is the output and its R class

  * maybe one example

makes the grief of re-discovering the function and trying to decipher it go away. What does this look like? Here's a recent example from my files:

````
#' Find column name corresponding to a particular functional
#'
#' The original data set contains very long column headers. This function
#' does a keyword search over the headers to find those column headers that
#' match a particular keyword, e.g., mean, median, etc.
#' @param x The data we are querying (data.frame)
#' @param v The keyword we are searching for (character)
#' @param ignorecase Should case be ignored (logical)
#' @return A vector of column names matching the keyword
#' @export
findvar <- function(x,v, ignorecase=TRUE) {
  if(!is.character(v)) stop('name must be character')
  if(!is.data.frame(x)) stop('x must be a data.frame')
  v <- grep(v,names(x),value=T, ignore.case=ignorecase)
  if(length(v)==0) v <- NA
  return(v)
}
````

My code above might not meet best practices, but it achieves two things for me. It reminds me of why I wrote this function, and tells me what I need to run it. This particular snippet is not part of any R package (though I could, with my new directory structure for projects, easily create a project-specific package if I need to). Of course this type of documentation is required if you are indeed writing packages.

**Update**: As some of you have pointed out, the way I'm using this is as a fancy form of commenting, regardless of future utility in packaging. 100% true, but it's actually one less thing for me to think about. I have a template, fill it out, and I'm done, with all the essential elements included. Essentially this creates a "minimal viable comment" for a function, and I only need to look in one place later to see what's going on. I still comment my code, but this still gives me value for not very much overhead.

### Resources

There are several resources for learning about roxygen2. First and foremost is the chapter [Documenting functions](http://adv-r.had.co.nz/Documenting-functions.html) from Hadley Wickham's [online book](http://adv-r.had.co.nz/). roxygen2 also has its own [tag](http://stackoverflow.com/questions/tagged/roxygen2) on StackOverflow.

On the software side, RStudio supports roxygen2; see [here](http://www.rstudio.com/ide/docs/packages/documentation). Emacs/ESS also has extensive roxygen2 support. The [Rtools](https://github.com/karthik/Rtools) package for Sublime Text provides a template for roxygen2 documentation. So getting started in the editor of your choice is not a problem.
